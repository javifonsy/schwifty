.. _changelog:

Changelog
=========

Versions follow `CalVer <http://www.calver.org/>`_ with the scheme ``YY.0M.Micro``.

`2023.11.2`_ - 2023/11/27
-------------------------
Added
~~~~~
* Add OKALI to the list of French banks `@Natim <https://github.com/Natim>`_.

`2023.11.1`_ - 2023/11/27
-------------------------
Changed
~~~~~~~
* The Swiss bank registry is now generated from the SIX Group.
* Manually add missing bank entry for Spain.
* Updated bank registr for Austria and Poland.

`2023.11.0`_ - 2023/11/17
-------------------------
Changed
~~~~~~~
* The validation of a :class:`.BIC` is now performed in the context of ISO 9362:2022 which allows
  numbers in the business party prefix. If strict SWIFT compliance is reqruied the
  ``enforce_swift_compliance`` parameter can be set to ``True``.
* The :meth:`.BIC.from_bank_code`-method will now select the most generic BIC (e.g. with no branch
  specifier or the "XXX" value) if multiple BICs are associated to the given domestic bank code.
  `@Natim <https://github.com/Natim>`_.
* Many manually curated bank registry entries have been re-added by `@dennisxtria <https://github.com/dennisxtria>`_

`2023.10.0`_ - 2023/10/31
-------------------------
Added
~~~~~~~
* The Pydantic v2 protocol is now supported, so that the :class:`.IBAN` and :class:`.BIC` classes
  can be directly used as type annotations in `Pydantic models <https://docs.pydantic.dev/latest/concepts/models/#basic-model-usage>`_

Changed
~~~~~~~
* The :class:`.IBAN` and :class:`.BIC` classes are now subclasses of :class:`str` so that all string
  related methods and functionallities (e.g. slicing) are directly available.

`2023.09.0`_ - 2023/09/25
-------------------------
Removed
~~~~~~~
* Support for Python 3.7 has been dropped.

Added
~~~~~
* New method :meth:`.BIC.candidates_from_bank_code` to list all matching BICs to a given domestic
  bank code `@Natim <https://github.com/Natim>`_.

Changed
~~~~~~~
* The Italian bank registry is now automatically generated thanks to
  `@Krystofee <https://github.com/Krystofee>`_

Internal
~~~~~~~~
* Switch project tooling to `hatch <https://hatch.pypa.io/latest/>`_.
* Use `ruff <https://docs.astral.sh/ruff/>`_ instead of [flake8](https://flake8.pycqa.org/en/latest/)
  as linter.
* Upgrade `mypy <https://www.mypy-lang.org/>`_ to 1.5.1 and fix all new typing errors.

`2023.06.0`_ - 2023/06/21
-------------------------
Fixed
~~~~~
* For Ukrainian banks calling ``iban.bic`` did result in a ``TypeError``. Thanks
  `@bernoreitsma <https://github.com/bernoreitsma>`_ for reporting.

Changed
~~~~~~~
* Updated generated bank registries for Austria, Belgium, Czech Republic, Germany, Netherlands,
  Hungary, Norway, Poland and Ukraine.


`2023.03.0`_ - 2023/03/14
-------------------------
Changed
~~~~~~~
* Updated generated bank registries for Austria, Belgium, Germany, Netherlands,
  Hungary, Slovenia and Ukraine.

Added
~~~~~
* New bank registry for Norway thanks to `@ezet <https://github.com/ezet>`_

`2023.02.1`_ - 2023/02/28
-------------------------
Fixed
~~~~~
* The domestic checksum calculation for Belgium now returns 97 in case the modulo operation
  results in 0. `@mhemeryck <https://github.com/mhemeryck>`_

Changed
~~~~~~~
* Updated generated bank registries for Austria, Belgium, Czech Republic, Germany, Spain,
  Hungary and Croatia.

`2023.02.0`_ - 2023/02/06
-------------------------
Added
~~~~~
* New banks for Portugal and Italy `@dennisxtria <https://github.com/dennisxtria>`_
* Added support for Ukrainian banks `@shpigunov <https://github.com/shpigunov>`_

Fixed
~~~~~
* Corrected bank codes for Cypriot banks `@Krystofee <https://github.com/Krystofee>`_

`2022.09.0`_ - 2022/16/09
-------------------------
Added
~~~~~
* IBAN validation for Senegal `mkopec87 <https://github.com/mkopec87>`_

Changed
~~~~~~~
* Refactored most of the scripts to generate the bank registry to use Pandas `@pebosi <https://github.com/pebosi>`_
* Updated bank registry for Austria, Belgium, Germany, Spain, Hungary, Netherlands and Poland.

`2022.07.1`_ - 2022/28/07
-------------------------
Fixed
~~~~~
* In some countries the BBAN does not include a bank code, but only a branch code (e.g. Poland). In
  those cases the branch code should be used to lookup the bank associated to an IBAN instead of the
  obviously empty bank code.

`2022.07.0`_ - 2022/07/07
-------------------------
Fixed
~~~~~
* Hungarian bank registry generator script was fixed by `@Krystofee <https://github.com/Krystofee>`_

`2022.06.3`_ - 2022/06/29
-------------------------
Added
~~~~~
* Generated list of Lithuanian BICs `@Draugelis <https://github.com/Draugelis>`_
* Removed manually curated list of Lithuanian banks.

`2022.06.2`_ - 2022/06/22
-------------------------
Added
~~~~~
* Generated list of Greek BICs `@kounabi  <https://github.com/kounabi>`_
* Generated list of Cypriot BICs `@kounabi  <https://github.com/kounabi>`_

Changed
~~~~~~~
* Updated bank registry for Austria, Belgium, Czech Republic, Germany, Croatia, Netherlands, Poland
  and Slovenia.

Fixed
~~~~~
* The domestic bank code for Hungarian banks was wrongly generated `@Krystofee <https://github.com/Krystofee>`_

`2022.06.1`_ - 2022/06/06
-------------------------

Added
~~~~~
* Generated list of Romanian BICs `@Krystofee <https://github.com/Krystofee>`_
* Generated list of Hungarian BICs `@Krystofee <https://github.com/Krystofee>`_
* Extended manually curated list of Irish BICs `@dennisxtria <https://github.com/dennisxtria>`_


`2022.06.0`_ - 2022/06/06
-------------------------

Added
~~~~~
* Manually curated list of Bulgarian BICs `@Krystofee <https://github.com/Krystofee>`_
* Manually curated list of Saudi Arabian BICs `@samizaman <https://github.com/samizaman>`_
* Support for `PyInstaller <https://pyinstaller.org/en/stable/>`_ `@Lukasz87 <https://github.com/Lukasz87>`_

Internal
~~~~~~~~
* Run tests on Python 3.10 `@adamchainz <https://github.com/adamchainz>`_
* Use standard keys in ``setup.cfg`` `@adamchainz <https://github.com/adamchainz>`_
* Don't rely on ``hacking`` in test-setup `@adamchainz <https://github.com/adamchainz>`_

`2022.04.2`_ - 2022/04/29
-------------------------

Changed
~~~~~~~
* Allow getting bank names from IBAN. Previously, you could do ``iban.bic.bank_names[0]``, but since
  a BIC can be associated to multiple bank codes the context of the specific bank is lost and you
  could end up with the wrong bank name. `@jose-reveni <https://github.com/jose-reveni>`_


`2022.04.1`_ - 2022/04/29
-------------------------

Changed
~~~~~~~
* The Italian BBAN checksum algorithm is now also applied for San Marino `@fabienpe <https://github.com/fabienpe>`_

Fixed
~~~~~
* Fix Italian BBAN checksum calculation `#78 <https://github.com/mdomke/schwifty/issues/78>`_
* Fix bank code position in BBAN for Jordan banks `@fabienpe <https://github.com/fabienpe>`_


`2022.04.0`_ - 2022/04/11
-------------------------

Changed
~~~~~~~
* Update bank registry for Austria, Czech Republic, Germany, Spain, Poland and Slovakia.

Fixed
~~~~~
* Removed bogus line from dutch bank registry.
* Loading the bank registry now also works on machines that don't have UTF-8 as their default
  encoding `@imad3v <https://github.com/imad3v>`_


`2022.03.1`_ - 2022/03/05
-------------------------

Added
~~~~~
* Country specifc checksum validation for French banks (based on the work of
  `@sholan <https://github.com/sholan>`_)


`2022.03.0`_ - 2022/03/04
-------------------------

Added
~~~~~
* The :class:`.IBAN` and :class:`.BIC` classes now support the ``__len__`` method to allow a more
  Pythonic calculation of the length.

Changed
~~~~~~~
* Update bank registry for Czech Republic, Spain, Hungary, Poland and Slovakia.


`2022.02.0`_ - 2022/02/15
-------------------------

Added
~~~~~
* N26 BIC for Spain `@brunovila <https://github.com/brunovila>`_
* Manually curated entries for banks from Iceland `@gautinils <https://github.com/gautinils>`_

Changed
~~~~~~~
* Removed manually curated bank entries for Spain since all values were already part of
  the generated registry.
* Updated bank registry for Austria, Belgium, Czech Republic, Germany, Spain, Netherlands and Poland
* Added overwrite for IBAN spec of Czech Republic and France. The branch and account code positions
  are wrongly provided in the official IBAN registry.

`2021.10.2`_ - 2021/10/12
-------------------------

Added
~~~~~
* Added 440 additional bank records for Spain.

`2021.10.1`_ - 2021/10/11
-------------------------

Changed
~~~~~~~
* Use `importlib.resources <https://docs.python.org/3.9/library/importlib.html#module-importlib.resources>`_
  for loading internal registries. This removes the need to have ``setuptools`` installed.
  Thank you `@a-recknagel <https://github.com/a-recknagel>`_ for the idea!

Fixed
~~~~~
* Ensure that Belgian BBAN checksums are always 2 digits long.

`2021.10.0`_ - 2021/10/01
-------------------------

Added
~~~~~
* Added IBAN spec for Sudan (SD).
* Added and extended manually curated bank entries for Turkey, Italy, Israel, Ireland, Spain,
  Switzerland and Denmark `@howorkon <https://github.com/howorkon>`_.

Changed
~~~~~~~
* Updated bank registry for Austria, Belgium, Czech Republic, Germany, Netherlands, Poland,
  Slovenia and Slovakia.

Fixed
~~~~~
* Disallow ``schwifty`` to be installed for Python versions older than 3.7. It was unsupported
  before but is now rejected upon installation with an appropriate error message.
* Austrian bank codes are now consistently left padded with zeros. This fixes the mapping from
  IBAN to BIC for the Austrian federal bank institutes.

`2021.06.1`_ - 2021/06/24
-------------------------

Added
~~~~~
* Enable tool based type checking as described in `PEP-0561`_ by adding the ``py.typed`` marker
  `@jmfederico <https://github.com/jmfederico>`_


`2021.06.0`_ - 2021/06/17
-------------------------

Added
~~~~~
* Added bank registry for Swedish Banks `@jmfederico <https://github.com/jmfederico>`_


`2021.05.2`_ - 2021/05/23
-------------------------

Added
~~~~~
* Country specifc checksum validation for Belgian banks, as well as support for generating the
  checksum when using the :meth:`.IBAN.generate`-method. `@mhemeryck <https://github.com/mhemeryck>`_

`2021.05.1`_ - 2021/05/20
-------------------------

Added
~~~~~
* The IBAN validation now optionally includes the verification of the country specific checksum
  within the BBAN. This currently works for German and Italian banks. For German banks the checksum
  algorithm for the account code is chosen by the bank code. Since there are over 150 bank specific
  algorithms in Germany not all of them are implemented at the moment, but the majority of banks
  should be covered.

Changed
~~~~~~~
* Update bank registry for Germany, Poland, Czech Republic, Austria and Netherlands.

`2021.05.0`_ - 2021/05/02
-------------------------

Added
~~~~~
* Added manually curated list of Lithuanian Banks (e.g Revolut Payments UAB).

`2021.04.0`_ - 2021/04/23
-------------------------

Changed
~~~~~~~
* Added type hints to the entire code base.
* Dropped support for Python 3.6
* Update bank registry for Austria, Poland, Germany, Belgium, Czech Republic, Netherlands, Slovenia
  and Slovakia.

`2021.01.0`_ - 2021/01/20
-------------------------

Changed
~~~~~~~
* Restructure documentation and change theme to `furo <https://pradyunsg.me/furo/>`_.
* Added dedicated exception classes for various validation errors.
* Drop support for Python 2. Only Python 3.6+ will be supported from now on.
* Use PEP 517/518 compliant build setup.

`2020.11.0`_ - 2020/12/02
-------------------------

Changed
~~~~~~~
* Updated IBAN registry and bank registries of Poland, Germany, Austria, Belgium, Netherlands,
  Czech Republic and Slovenia.

Added
~~~~~
* Added generated banks for Slovakia `@petrboros <https://github.com/petrboros>`_.
* Added a test to validate the correctnes of BICs in the registry `@ckoehn <https://github.com/ckoehn>`_.

Fixed
~~~~~
* Fixed encoding for Polish bank registry `@michal-michalak <https://github.com/michal-michalak>`_.

`2020.09.0`_ - 2020/09/07
-------------------------

Changed
~~~~~~~
* Migrated build and test pipelines to GitHub actions.

Added
~~~~~
* Added generated banks for Netherlands `@insensitiveclod <https://github.com/insensitiveclod>`_.
* Added generated banks for Spain.

`2020.08.3`_ - 2020/08/31
-------------------------

Fixed
~~~~~
* Fixed IBAN generation for countries with branch/sort code
* Add generated banks for Spain

`2020.08.2`_ - 2020/08/30
-------------------------

Fixed
~~~~~
* Poland's IBAN spec only has a branch-code but no bank-code
* Fixed listing of supported countries for BIC derivation.
* Fixed bank registry for Hungary.

Changed
~~~~~~~
* Updated bank registry Poland, Belgium and Austria.
* Updated IBAN spec for Sao Tome and Principe

`2020.08.1`_ - 2020/08/28
-------------------------

Added
~~~~~
* New attribute :attr:`.BIC.is_valid` and :attr:`.IBAN.is_valid`.

`2020.08.0`_ - 2020/08/06
-------------------------

Changed
~~~~~~~
* Updated bank registry for Poland.

`2020.05.3`_ - 2020/05/25
-------------------------

Added
~~~~~
* Added banks for France, Switzerland and Great Britain.

`2020.05.2`_ - 2020/05/08
-------------------------

Added
~~~~~
* Added :attr:`.BIC.country` and :attr:`.IBAN.country`.


.. _2023.11.2: https://github.com/mdomke/schwifty/compare/2023.11.1...2023.11.2
.. _2023.11.1: https://github.com/mdomke/schwifty/compare/2023.11.0...2023.11.1
.. _2023.11.0: https://github.com/mdomke/schwifty/compare/2023.10.0...2023.11.0
.. _2023.10.0: https://github.com/mdomke/schwifty/compare/2023.09.0...2023.10.0
.. _2023.09.0: https://github.com/mdomke/schwifty/compare/2023.06.0...2023.09.0
.. _2023.06.0: https://github.com/mdomke/schwifty/compare/2023.03.0...2023.06.0
.. _2023.03.0: https://github.com/mdomke/schwifty/compare/2023.02.1...2023.03.0
.. _2023.02.1: https://github.com/mdomke/schwifty/compare/2023.02.0...2023.02.1
.. _2023.02.0: https://github.com/mdomke/schwifty/compare/2022.09.0...2023.02.0
.. _2022.09.0: https://github.com/mdomke/schwifty/compare/2022.07.1...2022.09.0
.. _2022.07.1: https://github.com/mdomke/schwifty/compare/2022.07.0...2022.07.1
.. _2022.07.0: https://github.com/mdomke/schwifty/compare/2022.06.3...2022.07.0
.. _2022.06.3: https://github.com/mdomke/schwifty/compare/2022.06.2...2022.06.3
.. _2022.06.2: https://github.com/mdomke/schwifty/compare/2022.06.1...2022.06.2
.. _2022.06.1: https://github.com/mdomke/schwifty/compare/2022.06.0...2022.06.1
.. _2022.06.0: https://github.com/mdomke/schwifty/compare/2022.04.2...2022.06.0
.. _2022.04.2: https://github.com/mdomke/schwifty/compare/2022.04.1...2022.04.2
.. _2022.04.1: https://github.com/mdomke/schwifty/compare/2022.04.0...2022.04.1
.. _2022.04.0: https://github.com/mdomke/schwifty/compare/2022.03.1...2022.04.0
.. _2022.03.1: https://github.com/mdomke/schwifty/compare/2022.03.0...2022.03.1
.. _2022.03.0: https://github.com/mdomke/schwifty/compare/2022.02.0...2022.03.0
.. _2022.02.0: https://github.com/mdomke/schwifty/compare/2021.10.2...2022.02.0
.. _2021.10.2: https://github.com/mdomke/schwifty/compare/2021.10.1...2021.10.2
.. _2021.10.1: https://github.com/mdomke/schwifty/compare/2021.10.0...2021.10.1
.. _2021.10.0: https://github.com/mdomke/schwifty/compare/2021.06.1...2021.10.0
.. _2021.06.1: https://github.com/mdomke/schwifty/compare/2021.06.0...2021.06.1
.. _2021.06.0: https://github.com/mdomke/schwifty/compare/2021.05.2...2021.06.0
.. _2021.05.2: https://github.com/mdomke/schwifty/compare/2021.05.1...2021.05.2
.. _2021.05.1: https://github.com/mdomke/schwifty/compare/2021.05.0...2021.05.1
.. _2021.05.0: https://github.com/mdomke/schwifty/compare/2021.04.0...2021.05.0
.. _2021.04.0: https://github.com/mdomke/schwifty/compare/2021.01.0...2021.04.0
.. _2021.01.0: https://github.com/mdomke/schwifty/compare/2020.11.0...2021.01.0
.. _2020.11.0: https://github.com/mdomke/schwifty/compare/2020.09.0...2020.11.0
.. _2020.09.0: https://github.com/mdomke/schwifty/compare/2020.08.3...2020.09.0
.. _2020.08.3: https://github.com/mdomke/schwifty/compare/2020.08.2...2020.08.3
.. _2020.08.2: https://github.com/mdomke/schwifty/compare/2020.08.1...2020.08.2
.. _2020.08.1: https://github.com/mdomke/schwifty/compare/2020.08.0...2020.08.1
.. _2020.08.0: https://github.com/mdomke/schwifty/compare/2020.05.3...2020.08.0
.. _2020.05.3: https://github.com/mdomke/schwifty/compare/2020.05.2...2020.05.3
.. _2020.05.2: https://github.com/mdomke/schwifty/compare/2020.05.1...2020.05.2

.. _PEP-0561: https://www.python.org/dev/peps/pep-0561/#packaging-type-information
