``schwifty`` by example
=======================

Basics
------

:class:`.IBAN`-objects are usually created from their string representation

.. code-block:: pycon

  >>> from schwifty import IBAN
  >>> iban = IBAN('DE89 3704 0044 0532 0130 00')
  <IBAN=DE89370400440532013000>


Afterwards you can access all relevant components and meta-information of the IBAN as attributes.

.. code-block:: pycon

  >>> str(iban)
  'DE89370400440532013000'
  >>> iban.formatted
  'DE89 3704 0044 0532 0130 00'
  >>> iban.country_code
  'DE'
  >>> iban.bank_code
  '37040044'
  >>> iban.account_code
  '0532013000'
  >>> len(iban)
  22

For some countries it is also possible to get ahold of the :class:`.BIC` associated to the bank-code
of the IBAN.

.. code-block:: pycon

  >>> iban.bic
  <BIC=COBADEFFXXX>

A BIC is a unique identification code for both financial and non-financial institutes. ``schwifty``
provides a :class:`.BIC`-object, that has a similar interface to the :class:`.IBAN`.

.. code-block:: pycon

  >>> from schwifty import BIC
  >>> bic = BIC('PBNKDEFFXXX')
  >>> bic.bank_code
  'PBNK'
  >>> bic.branch_code
  'XXX'
  >>> bic.country_code
  'DE'
  >>> bic.location_code
  'FF'
  >>> bic.domestic_bank_codes
  ['10010010',
   '20010020',
   ...
   '86010090']

The :attr:`.BIC.domestic_bank_codes` lists the country specific bank codes as you can find them as
part of the IBAN. This mapping is included in a manually curated registry that ships with ``schwifty``.
and currently includes entries for the following countries:

* Austria
* Belgium
* Bulgaria
* Croatia
* Czech Republic
* Finland
* France
* Germany
* Great Britan
* Hungary
* Ireland
* Latvia
* Lithuania
* Netherlands
* Poland
* Romania
* Saudi Arabia
* Slovakia
* Slovenia
* Spain
* Sweden
* Switzerland

.. note::

  The :class:`.IBAN` and :class:`.BIC` classes are subclasses of :class:`str` so that all methods
  and functionallities (e.g. slicing) can be directly used. E.g.

  .. code-block:: pycon

    >>> iban = IBAN('DE89 3704 0044 0532 0130 00')
    >>> iban[2:4]
    "89"
    >>> iban.count("0")
    8
    >>> iban.startswith("DE")
    True


Validation
----------

When it comes to validation the :class:`.IBAN` and :class:`.BIC` constructors raise an exception
whenever the provided code is incorrect for some reason. ``schwifty`` comes with a number of
dedicated exceptions classes that help identify the concrete reason for the validation error. They
all derive from a common base exception :exc:`.SchwiftyException` which makes it easy to catch all
validation failures if the concrete cause is not important to you.

.. note::

   Prior to schwifty 2021.01.0 a ``ValueError`` was raised for all kind of validation failures. In
   order to keep backwards compatiblity schwifty's base exception is a subclass of ``ValueError``.

For IBANs - with respect to ISO 13616 compliance - it is checked if the account-code, the bank-code
and possibly the branch-code have the correct country-specific format. E.g.:

.. code-block:: pycon

  >>> IBAN('DX89 3704 0044 0532 0130 00')
  ...
  InvalidCountryCode: Unknown country-code DX

  >>> IBAN('DE99 3704 0044 0532 0130 00')
  ...
  InvalidChecksumDigits: Invalid checksum digits

Since version 2021.05.1 ``schwifty`` also provides the ability to validate the country specific
checksum within the BBAN. This currently works for German and Italian banks. For German IBANs the
bank specific checksum algorithm for the account code is derived from the bank code. This
functionality is currently opt-in and can be used by providing the `validate_bban` paramter to the
:class:`.IBAN` constructor or the :meth:`.IBAN.validate`-method.

.. code-block:: pycon

   >>> iban = IBAN('DE20 2909 0900 8840 0170 00')
   >>> iban.validate(validate_bban=True)
   ...
   InvalidBBANChecksum: Invalid BBAN checksum

   >>> IBAN('DE20 2909 0900 8840 0170 00', validate_bban=True)
   ...
   InvalidBBANChecksum: Invalid BBAN checksum

For BICs it is checked if the country-code and the length is valid and if the structure matches the
ISO 9362 specification.

.. code-block:: pycon

  >>> BIC('PBNKDXFFXXX')
  ...
  InvalidCountryCode: Invalid country code DX

  >>> BIC('PBNKDXFFXXXX')
  ...
  InvalidLength: Invalid length 12

  >>> BIC('PBNKD1FFXXXX')
  ...
  InvalidStructure: Invalid structure PBN1DXFFXXXX

.. note::

  Starting from schwifty 2023.11.0 BIC values are being validated in the context of ISO 9362:2022
  which allows numbers to be part of the business prefix (the first 4 characters of the BIC). The
  SWIFT however still enforces alphabetic characters only. If strict SWIFT compliance is required
  you can use the ``enforce_swift_compliance``-parameter, e.g.

  .. code-block:: pycon

    >>> BIC("1234DEWWXXX", enforce_swift_compliance=True)
    ...
    InvalidStructure: Invalid structure 1234DEWWXXX

If catching an exception would complicate your code flow you can also use the :attr:`.IBAN.is_valid`
property. E.g.:

.. code-block:: python

  if IBAN(value, allow_invalid=True).is_valid:
    # do something with value


Generation
----------

You can generate :class:`.IBAN`-objects from country-code, bank-code and account-number by using the
:meth:`.IBAN.generate()`-method. It will automatically calculate the correct checksum digits for
you.

.. code-block:: pycon

  >>> iban = IBAN.generate('DE', bank_code='10010010', account_code='12345')
  <IBAN=DE40100100100000012345>
  >>> iban.checksum_digits
  '40'

Notice that even that the account-code has less digits than required (in Germany accounts should be
10 digits long), zeros have been added at the correct location.

For some countries you can also generate :class:`.BIC`-objects from local
bank-codes, e.g.:

.. code-block:: pycon

  >>> bic = BIC.from_bank_code('DE', '43060967')
  >>> bic.formatted
  'GENO DE M1 GLS'

In case there are multiple BICs that can be related to a domestic bank code you can also use the
:meth:`.BIC.candidates_from_bank_code`-method to get a list of all knwon BIC candidates.

.. code-block:: pycon

  >>> BIC.candidates_from_bank_code('FR', '30004') # doctest: +NORMALIZE_WHITESPACE
  [<BIC=BNPAFRPPIFN>, <BIC=BNPAFRPPPAA>, <BIC=BNPAFRPPMED>, <BIC=BNPAFRPPCRN>,
   <BIC=BNPAFRPP>, <BIC=BNPAFRPPPAE>, <BIC=BNPAFRPPPBQ>, <BIC=BNPAFRPPNFE>,
   <BIC=BNPAFRPPPGN>, <BIC=BNPAFRPPXXX>, <BIC=BNPAFRPPBOR>, <BIC=BNPAFRPPCRM>,
   <BIC=BNPAFRPPPVD>, <BIC=BNPAFRPPPTX>, <BIC=BNPAFRPPPAC>, <BIC=BNPAFRPPPLZ>,
   <BIC=BNPAFRPP039>, <BIC=BNPAFRPPENG>, <BIC=BNPAFRPPNEU>, <BIC=BNPAFRPPORE>,
   <BIC=BNPAFRPPPEE>, <BIC=BNPAFRPPPXV>, <BIC=BNPAFRPPIFO>]


Pydantic integration
---------------------

The :class:`.IBAN` and :class:`.BIC` types can be directly used for the popular data validation
library `Pydantic <https://docs.pydantic.dev/latest/>`_ like so

.. code-block:: python

  from pydantic import BaseModel
  from schwifty import IBAN


  class Model(BaseModel):
    iban: IBAN

  model = Model(iban="DE89370400440532013000")  # OK
  model = Model(iban="DX89370400440532013000")  # Raises ValidationError due to invalid country code
