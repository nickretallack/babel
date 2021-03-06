.. -*- mode: rst; encoding: utf-8 -*-

===============
Date Formatting
===============


When working with date and time information in Python, you commonly use the
classes ``date``, ``datetime`` and/or ``time`` from the `datetime`_ package.
Babel provides functions for locale-specific formatting of those objects in its
``dates`` module:

.. _`datetime`: http://docs.python.org/lib/module-datetime.html

.. code-block:: pycon

    >>> from datetime import date, datetime, time
    >>> from babel.dates import format_date, format_datetime, format_time

    >>> d = date(2007, 4, 1)
    >>> format_date(d, locale='en')
    u'Apr 1, 2007'
    >>> format_date(d, locale='de_DE')
    u'01.04.2007'

As this example demonstrates, Babel will automatically choose a date format
that is appropriate for the requested locale.

The ``format_*()`` functions also accept an optional ``format`` argument, which
allows you to choose between one of four format variations:

 * ``short``,
 * ``medium`` (the default),
 * ``long``, and
 * ``full``.

For example:

.. code-block:: pycon

    >>> format_date(d, format='short', locale='en')
    u'4/1/07'
    >>> format_date(d, format='long', locale='en')
    u'April 1, 2007'
    >>> format_date(d, format='full', locale='en')
    u'Sunday, April 1, 2007'


Pattern Syntax
==============

While Babel makes it simple to use the appropriate date/time format for a given
locale, you can also force it to use custom patterns. Note that Babel uses
different patterns for specifying number and date formats compared to the
Python equivalents (such as ``time.strftime()``), which have mostly been
inherited from C and POSIX. The patterns used in Babel are based on the
`Locale Data Markup Language specification`_ (LDML), which defines them as
follows:

    A date/time pattern is a string of characters, where specific strings of
    characters are replaced with date and time data from a calendar when formatting
    or used to generate data for a calendar when parsing. […]

    Characters may be used multiple times. For example, if ``y`` is used for the
    year, ``yy`` might produce "99", whereas ``yyyy`` produces "1999". For most
    numerical fields, the number of characters specifies the field width. For
    example, if ``h`` is the hour, ``h`` might produce "5", but ``hh`` produces
    "05". For some characters, the count specifies whether an abbreviated or full
    form should be used […]

    Two single quotes represent a literal single quote, either inside or outside
    single quotes. Text within single quotes is not interpreted in any way (except
    for two adjacent single quotes).

For example:

.. code-block:: pycon

    >>> d = date(2007, 4, 1)
    >>> format_date(d, "EEE, MMM d, ''yy", locale='en')
    u"Sun, Apr 1, '07"
    >>> format_date(d, "EEEE, d.M.yyyy", locale='de')
    u'Sonntag, 1.4.2007'

    >>> t = time(15, 30)
    >>> format_time(t, "hh 'o''clock' a", locale='en')
    u"03 o'clock PM"
    >>> format_time(t, 'H:mm a', locale='de')
    u'15:30 nachm.'

    >>> dt = datetime(2007, 4, 1, 15, 30)
    >>> format_datetime(dt, "yyyyy.MMMM.dd GGG hh:mm a", locale='en')
    u'02007.April.01 AD 03:30 PM'

The syntax for custom datetime format patterns is described in detail in the
the `Locale Data Markup Language specification`_. The following table is just a
relatively brief overview.

 .. _`Locale Data Markup Language specification`: http://unicode.org/reports/tr35/#Date_Format_Patterns

Date Fields
-----------

  +----------+--------+--------------------------------------------------------+
  | Field    | Symbol | Description                                            |
  +==========+========+========================================================+
  | Era      | ``G``  | Replaced with the era string for the current date. One |
  |          |        | to three letters for the abbreviated form, four        |
  |          |        | lettersfor the long form, five for the narrow form     |
  +----------+--------+--------------------------------------------------------+
  | Year     | ``y``  | Replaced by the year. Normally the length specifies    |
  |          |        | the padding, but for two letters it also specifies the |
  |          |        | maximum length.                                        |
  |          +--------+--------------------------------------------------------+
  |          | ``Y``  | Same as ``y`` but uses the ISO year-week calendar.     |
  |          +--------+--------------------------------------------------------+
  |          | ``u``  | ??                                                     |
  +----------+--------+--------------------------------------------------------+
  | Quarter  | ``Q``  | Use one or two for the numerical quarter, three for    |
  |          |        | the abbreviation, or four for the full name.           |
  |          +--------+--------------------------------------------------------+
  |          | ``q``  | Use one or two for the numerical quarter, three for    |
  |          |        | the abbreviation, or four for the full name.           |
  +----------+--------+--------------------------------------------------------+
  | Month    | ``M``  | Use one or two for the numerical month, three for the  |
  |          |        | abbreviation, or four for the full name, or five for   |
  |          |        | the narrow name.                                       |
  |          +--------+--------------------------------------------------------+
  |          | ``L``  | Use one or two for the numerical month, three for the  |
  |          |        | abbreviation, or four for the full name, or 5 for the  |
  |          |        | narrow name.                                           |
  +----------+--------+--------------------------------------------------------+
  | Week     | ``w``  | Week of year.                                          |
  |          +--------+--------------------------------------------------------+
  |          | ``W``  | Week of month.                                         |
  +----------+--------+--------------------------------------------------------+
  | Day      | ``d``  | Day of month.                                          |
  |          +--------+--------------------------------------------------------+
  |          | ``D``  | Day of year.                                           |
  |          +--------+--------------------------------------------------------+
  |          | ``F``  | Day of week in month.                                  |
  |          +--------+--------------------------------------------------------+
  |          | ``g``  | ??                                                     |
  +----------+--------+--------------------------------------------------------+
  | Week day | ``E``  | Day of week. Use one through three letters for the     |
  |          |        | short day, or four for the full name, or five for the  |
  |          |        | narrow name.                                           |
  |          +--------+--------------------------------------------------------+
  |          | ``e``  | Local day of week. Same as E except adds a numeric     |
  |          |        | value that will depend on the local starting day of    |
  |          |        | the week, using one or two letters.                    |
  |          +--------+--------------------------------------------------------+
  |          | ``c``  | ??                                                     |
  +----------+--------+--------------------------------------------------------+

Time Fields
-----------

  +----------+--------+--------------------------------------------------------+
  | Field    | Symbol | Description                                            |
  +==========+========+========================================================+
  | Period   | ``a``  | AM or PM                                               |
  +----------+--------+--------------------------------------------------------+
  | Hour     | ``h``  | Hour [1-12].                                           |
  |          +--------+--------------------------------------------------------+
  |          | ``H``  | Hour [0-23].                                           |
  |          +--------+--------------------------------------------------------+
  |          | ``K``  | Hour [0-11].                                           |
  |          +--------+--------------------------------------------------------+
  |          | ``k``  | Hour [1-24].                                           |
  +----------+--------+--------------------------------------------------------+
  | Minute   | ``m``  | Use one or two for zero places padding.                |
  +----------+--------+--------------------------------------------------------+
  | Second   | ``s``  | Use one or two for zero places padding.                |
  |          +--------+--------------------------------------------------------+
  |          | ``S``  | Fractional second, rounds to the count of letters.     |
  |          +--------+--------------------------------------------------------+
  |          | ``A``  | Milliseconds in day.                                   |
  +----------+--------+--------------------------------------------------------+
  | Timezone | ``z``  | Use one to three letters for the short timezone or     |
  |          |        | four for the full name.                                |
  |          +--------+--------------------------------------------------------+
  |          | ``Z``  | Use one to three letters for RFC 822, four letters for |
  |          |        | GMT format.                                            |
  |          +--------+--------------------------------------------------------+
  |          | ``v``  | Use one letter for short wall (generic) time, four for |
  |          |        | long wall time.                                        |
  |          +--------+--------------------------------------------------------+
  |          | ``V``  | Same as ``z``, except that timezone abbreviations      |
  |          |        | should be used regardless of whether they are in       |
  |          |        | common use by the locale.                              |
  +----------+--------+--------------------------------------------------------+


Time Delta Formatting
=====================

In addition to providing functions for formatting localized dates and times,
the ``babel.dates`` module also provides a function to format the difference
between two times, called a ''time delta''. These are usually represented as
``datetime.timedelta`` objects in Python, and it's also what you get when you
subtract one ``datetime`` object from an other.

The ``format_timedelta`` function takes a ``timedelta`` object and returns a
human-readable representation. This happens at the cost of precision, as it
chooses only the most significant unit (such as year, week, or hour) of the
difference, and displays that:

.. code-block:: pycon

    >>> from datetime import timedelta
    >>> from babel.dates import format_timedelta
    >>> delta = timedelta(days=6)
    >>> format_timedelta(delta, locale='en_US')
    u'1 week'

The resulting strings are based from the CLDR data, and are properly
pluralized depending on the plural rules of the locale and the calculated
number of units.

The function provides parameters for you to influence how this most significant
unit is chosen: with ``threshold`` you set the value after which the
presentation switches to the next larger unit, and with ``granularity`` you
can limit the smallest unit to display:

.. code-block:: pycon

    >>> delta = timedelta(days=6)
    >>> format_timedelta(delta, threshold=1.2, locale='en_US')
    u'6 days'
    >>> format_timedelta(delta, granularity='month', locale='en_US')
    u'1 month'


Time-zone Support
=================

Many of the verbose time formats include the time-zone, but time-zone
information is not by default available for the Python ``datetime`` and
``time`` objects. The standard library includes only the abstract ``tzinfo``
class, which you need appropriate implementations for to actually use in your
application. Babel includes a ``tzinfo`` implementation for UTC (Universal
Time).

Babel uses `pytz`_ for real timezone support which includes the
definitions of practically all of the time-zones used on the world, as
well as important functions for reliably converting from UTC to local
time, and vice versa.  The module is generally wrapped for you so you can
directly interface with it from within Babel:

.. code-block:: pycon

    >>> from datetime import time
    >>> from babel.dates import get_timezone, UTC
    >>> dt = datetime(2007, 04, 01, 15, 30, tzinfo=UTC)
    >>> eastern = get_timezone('US/Eastern')
    >>> format_datetime(dt, 'H:mm Z', tzinfo=eastern, locale='en_US')
    u'11:30 -0400'

The recommended approach to deal with different time-zones in a Python
application is to always use UTC internally, and only convert from/to the users
time-zone when accepting user input and displaying date/time data, respectively.
You can use Babel together with ``pytz`` to apply a time-zone to any
``datetime`` or ``time`` object for display, leaving the original information
unchanged:

.. code-block:: pycon

    >>> british = get_timezone('Europe/London')
    >>> format_datetime(dt, 'H:mm zzzz', tzinfo=british, locale='en_US')
    u'16:30 British Summer Time'

Here, the given UTC time is adjusted to the "Europe/London" time-zone, and
daylight savings time is taken into account. Daylight savings time is also
applied to ``format_time``, but because the actual date is unknown in that
case, the current day is assumed to determine whether DST or standard time
should be used.

For many timezones it's also possible to ask for the next timezone
transition.  This for instance is useful to answer the question “when do I
have to move the clock forward next”:

.. code-block:: pycon

    >>> t = get_next_timezone_transition('Europe/Vienna', datetime(2011, 3, 2))
    >>> t
    <TimezoneTransition CET -> CEST (2011-03-27 01:00:00)>
    >>> t.from_offset
    3600.0
    >>> t.to_offset
    7200.0
    >>> t.from_tz
    'CET'
    >>> t.to_tz
    'CEST'

 .. _`pytz`: http://pytz.sourceforge.net/


Localized Time-zone Names
-------------------------

While the ``Locale`` class provides access to various locale display names
related to time-zones, the process of building a localized name of a time-zone
is actually quite complicated. Babel implements it in separately usable
functions in the ``babel.dates`` module, most importantly the
``get_timezone_name`` function:

.. code-block:: pycon

    >>> from pytz import timezone
    >>> from babel import Locale
    >>> from babel.dates import get_timezone_name

    >>> tz = timezone('Europe/Berlin')
    >>> get_timezone_name(tz, locale=Locale.parse('pt_PT'))
    u'Hor\xe1rio Alemanha'

You can pass the function either a ``datetime.tzinfo`` object, or a
``datetime.date`` or ``datetime.datetime`` object. If you pass an actual date,
the function will be able to take daylight savings time into account. If you
pass just the time-zone, Babel does not know whether daylight savings time is
in effect, so it uses a generic representation, which is useful for example to
display a list of time-zones to the user.

.. code-block:: pycon

    >>> from datetime import datetime

    >>> dt = tz.localize(datetime(2007, 8, 15))
    >>> get_timezone_name(dt, locale=Locale.parse('de_DE'))
    u'Mitteleurop\xe4ische Sommerzeit'
    >>> get_timezone_name(tz, locale=Locale.parse('de_DE'))
    u'Deutschland'


Parsing Dates
=============

Babel can also parse date and time information in a locale-sensitive manner:

.. code-block:: pycon

    >>> from babel.dates import parse_date, parse_datetime, parse_time

.. note:: Date/time parsing is not properly implemented yet
