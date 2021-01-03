.. SPDX-License-Identifier: CC-BY-SA-4.0

.. Copyright (C) 1988, 2008 On-Line Applications Research Corporation (OAR)

Directives
==========

This section details the clock manager's directives.  A subsection is dedicated
to each of this manager's directives and describes the calling sequence,
related constants, usage, and status codes.

.. raw:: latex

   \clearpage

.. index:: set the time of day
.. index:: rtems_clock_set

.. _rtems_clock_set:

CLOCK_SET - Set date and time
-----------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_status_code rtems_clock_set(
            rtems_time_of_day *time_buffer
        );

DIRECTIVE STATUS CODES:
    .. list-table::
      :class: rtems-table

      * - ``RTEMS_SUCCESSFUL``
        - date and time set successfully
      * - ``RTEMS_INVALID_ADDRESS``
        - ``time_buffer`` is NULL
      * - ``RTEMS_INVALID_CLOCK``
        - invalid time of day

DESCRIPTION:
    This directive sets the system date and time.  The date, time, and ticks in
    the time_buffer structure are all range-checked, and an error is returned
    if any one is out of its valid range.

NOTES:
    Years before 1988 are invalid.

    The system date and time are based on the configured tick rate (number of
    microseconds in a tick).

    Setting the time forward may cause a higher priority task, blocked waiting
    on a specific time, to be made ready.  In this case, the calling task will
    be preempted after the next clock tick.

    Re-initializing RTEMS causes the system date and time to be reset to an
    uninitialized state.  Another call to ``rtems_clock_set`` is required to
    re-initialize the system date and time to application specific
    specifications.

.. raw:: latex

   \clearpage

.. index:: obtain the time of day
.. index:: rtems_clock_get_tod

.. _rtems_clock_get_tod:

CLOCK_GET_TOD - Get date and time in TOD format
-----------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_status_code rtems_clock_get_tod(
            rtems_time_of_day *time_buffer
        );

DIRECTIVE STATUS CODES:
    .. list-table::
      :class: rtems-table

      * - ``RTEMS_SUCCESSFUL``
	- current time obtained successfully
      * - ``RTEMS_NOT_DEFINED``
	- system date and time is not set
      * - ``RTEMS_INVALID_ADDRESS``
	- ``time_buffer`` is NULL

DESCRIPTION:
    This directive obtains the system date and time.  If the date and time has
    not been set with a previous call to ``rtems_clock_set``, then the
    ``RTEMS_NOT_DEFINED`` status code is returned.

NOTES:
    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.
    Re-initializing RTEMS causes the system date and time to be reset to an
    uninitialized state.  Another call to ``rtems_clock_set`` is required to
    re-initialize the system date and time to application specific
    specifications.

.. raw:: latex

   \clearpage

.. index:: obtain the time of day
.. index:: rtems_clock_get_tod_timeval

.. _rtems_clock_get_tod_timeval:

CLOCK_GET_TOD_TIMEVAL - Get date and time in timeval format
-----------------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_status_code rtems_clock_get_tod_interval(
            struct timeval  *time
        );

DIRECTIVE STATUS CODES:
    .. list-table::
      :class: rtems-table
      * - ``RTEMS_SUCCESSFUL``
	- current time obtained successfully
      * - ``RTEMS_NOT_DEFINED``
	- system date and time is not set
      * - ``RTEMS_INVALID_ADDRESS``
	- ``time`` is NULL

DESCRIPTION:
    This directive obtains the system date and time in POSIX ``struct timeval``
    format.  If the date and time has not been set with a previous call to
    ``rtems_clock_set``, then the ``RTEMS_NOT_DEFINED`` status code is
    returned.

NOTES:
    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.
    Re-initializing RTEMS causes the system date and time to be reset to an
    uninitialized state.  Another call to ``rtems_clock_set`` is required to
    re-initialize the system date and time to application specific
    specifications.

.. raw:: latex

   \clearpage

.. index:: obtain seconds since epoch
.. index:: rtems_clock_get_seconds_since_epoch

.. _rtems_clock_get_seconds_since_epoch:

CLOCK_GET_SECONDS_SINCE_EPOCH - Get seconds since epoch
-------------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_status_code rtems_clock_get_seconds_since_epoch(
            rtems_interval *the_interval
        );

DIRECTIVE STATUS CODES:
    .. list-table::
      :class: rtems-table
      * - ``RTEMS_SUCCESSFUL``
	- current time obtained successfully
      * - ``RTEMS_NOT_DEFINED``
	- system date and time is not set
      * - ``RTEMS_INVALID_ADDRESS``
	- ``the_interval`` is NULL

DESCRIPTION:
    This directive returns the number of seconds since the RTEMS epoch and the
    current system date and time.  If the date and time has not been set with a
    previous call to ``rtems_clock_set``, then the ``RTEMS_NOT_DEFINED`` status
    code is returned.

NOTES:
    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.
    Re-initializing RTEMS causes the system date and time to be reset to an
    uninitialized state.  Another call to ``rtems_clock_set`` is required to
    re-initialize the system date and time to application specific
    specifications.

.. raw:: latex

   \clearpage

.. index:: obtain seconds since epoch
.. index:: rtems_clock_get_ticks_per_second

.. _rtems_clock_get_ticks_per_second:

CLOCK_GET_TICKS_PER_SECOND - Get ticks per second
-------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_interval rtems_clock_get_ticks_per_second(void);

DIRECTIVE STATUS CODES:
    NONE

DESCRIPTION:
    This directive returns the number of clock ticks per second.  This is
    strictly based upon the microseconds per clock tick that the application
    has configured.

NOTES:
    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.

.. raw:: latex

   \clearpage

.. index:: obtain ticks since boot
.. index:: get current ticks counter value
.. index:: rtems_clock_get_ticks_since_boot

.. _rtems_clock_get_ticks_since_boot:

CLOCK_GET_TICKS_SINCE_BOOT - Get current ticks counter value
------------------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_interval rtems_clock_get_ticks_since_boot(void);

DIRECTIVE STATUS CODES:
    NONE

DESCRIPTION:

    This directive returns the current tick counter value.  With a 1ms clock
    tick, this counter overflows after 50 days since boot.  This is the
    historical measure of uptime in an RTEMS system.  The newer service
    ``rtems_clock_get_uptime`` is another and potentially more accurate way of
    obtaining similar information.

NOTES:

    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.

.. raw:: latex

   \clearpage

.. index:: rtems_clock_tick_later

.. _rtems_clock_tick_later:

CLOCK_TICK_LATER - Get tick value in the future
-----------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_interval rtems_clock_tick_later(
            rtems_interval delta
        );

DESCRIPTION:
    Returns the ticks counter value delta ticks in the future.

NOTES:
    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.

.. raw:: latex

   \clearpage

.. index:: rtems_clock_tick_later_usec

.. _rtems_clock_tick_later_usec:

CLOCK_TICK_LATER_USEC - Get tick value in the future in microseconds
--------------------------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_interval rtems_clock_tick_later_usec(
            rtems_interval delta_in_usec
        );

DESCRIPTION:
    Returns the ticks counter value at least delta microseconds in the future.

NOTES:
    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.

.. raw:: latex

   \clearpage

.. index:: rtems_clock_tick_before

.. _rtems_clock_tick_before:

CLOCK_TICK_BEFORE - Is tick value is before a point in time
-----------------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_interval rtems_clock_tick_before(
            rtems_interval tick
        );

DESCRIPTION:
    Returns true if the current ticks counter value indicates a time before the
    time specified by the tick value and false otherwise.

NOTES:
    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.

EXAMPLE:
    .. code-block:: c

        status busy( void )
        {
            rtems_interval timeout = rtems_clock_tick_later_usec( 10000 );
            do {
                if ( ok() ) {
                    return success;
                }
            } while ( rtems_clock_tick_before( timeout ) );
            return timeout;
        }

.. raw:: latex

   \clearpage

.. index:: clock get uptime
.. index:: uptime
.. index:: rtems_clock_get_uptime

.. _rtems_clock_get_uptime:

CLOCK_GET_UPTIME - Get the time since boot
------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        rtems_status_code rtems_clock_get_uptime(
            struct timespec *uptime
        );

DIRECTIVE STATUS CODES:
    .. list-table::
      :class: rtems-table
      * - ``RTEMS_SUCCESSFUL``
	- clock tick processed successfully
      * - ``RTEMS_INVALID_ADDRESS``
	- ``time_buffer`` is ``NULL``

DESCRIPTION:
    This directive returns the seconds and nanoseconds since the system was
    booted.  If the BSP supports nanosecond clock accuracy, the time reported
    will probably be different on every call.

NOTES:
    This directive may be called from an ISR.

.. raw:: latex

   \clearpage

.. index:: clock get uptime interval
.. index:: uptime
.. index:: rtems_clock_get_uptime_timeval

.. _rtems_clock_get_uptime_timeval:

CLOCK_GET_UPTIME_TIMEVAL - Get the time since boot in timeval format
--------------------------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        void rtems_clock_get_uptime_timeval(
            struct timeval *uptime
        );

DIRECTIVE STATUS CODES:
    NONE

DESCRIPTION:
    This directive returns the seconds and microseconds since the system was
    booted.  If the BSP supports nanosecond clock accuracy, the time reported
    will probably be different on every call.

NOTES:
    This directive may be called from an ISR.

.. raw:: latex

   \clearpage

.. index:: clock get uptime seconds
.. index:: uptime
.. index:: rtems_clock_get_uptime_seconds

.. _rtems_clock_get_uptime_seconds:

CLOCK_GET_UPTIME_SECONDS - Get the seconds since boot
-----------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        time_t rtems_clock_get_uptime_seconds(void);

DIRECTIVE STATUS CODES:
    The system uptime in seconds.

DESCRIPTION:
    This directive returns the seconds since the system was booted.

NOTES:
    This directive may be called from an ISR.

.. raw:: latex

   \clearpage

.. index:: clock get nanoseconds uptime
.. index:: uptime
.. index:: rtems_clock_get_uptime_nanoseconds

.. _rtems_clock_get_uptime_nanoseconds:

CLOCK_GET_UPTIME_NANOSECONDS - Get the nanoseconds since boot
-------------------------------------------------------------

CALLING SEQUENCE:
    .. code-block:: c

        uint64_t rtems_clock_get_uptime_nanoseconds(void);

DIRECTIVE STATUS CODES:
    The system uptime in nanoseconds.

DESCRIPTION:
    This directive returns the nanoseconds since the system was booted.

NOTES:
    This directive may be called from an ISR.

Removed Directives
==================

.. raw:: latex

   \clearpage

.. _rtems_clock_get:

CLOCK_GET - Get date and time information
-----------------------------------------
.. index:: obtain the time of day
.. index:: rtems_clock_get

.. warning::

    This directive was removed in RTEMS 5.1.  See also
    :ref:`ClockManagerAdviceClockGet`.

CALLING SEQUENCE:
    .. code-block:: c

        rtems_status_code rtems_clock_get(
           rtems_clock_get_options  option,
           void                    *time_buffer
        );

DIRECTIVE STATUS CODES:
    .. list-table::
      :class: rtems-table

      * - ``RTEMS_SUCCESSFUL``
        - current time obtained successfully
      * - ``RTEMS_NOT_DEFINED``
        - system date and time is not set
      * - ``RTEMS_INVALID_ADDRESS``
        - ``time_buffer`` is NULL

DESCRIPTION:
    This directive obtains the system date and time.  If the caller is
    attempting to obtain the date and time (i.e.  option is set to either
    ``RTEMS_CLOCK_GET_SECONDS_SINCE_EPOCH``, ``RTEMS_CLOCK_GET_TOD``, or
    ``RTEMS_CLOCK_GET_TIME_VALUE``) and the date and time has not been set with
    a previous call to ``rtems_clock_set``, then the ``RTEMS_NOT_DEFINED``
    status code is returned.  The caller can always obtain the number of ticks
    per second (option is ``RTEMS_CLOCK_GET_TICKS_PER_SECOND``) and the number
    of ticks since the executive was initialized option is
    ``RTEMS_CLOCK_GET_TICKS_SINCE_BOOT``).

    The ``option`` argument may taken on any value of the enumerated type
    ``rtems_clock_get_options``.  The data type expected for ``time_buffer`` is
    based on the value of ``option`` as indicated below:

    .. index:: rtems_clock_get_options

    +-----------------------------------------+---------------------------+
    | Option                                  | Return type               |
    +=========================================+===========================+
    | ``RTEMS_CLOCK_GET_TOD``                 | ``(rtems_time_of_day *)`` |
    +-----------------------------------------+---------------------------+
    | ``RTEMS_CLOCK_GET_SECONDS_SINCE_EPOCH`` | ``(rtems_interval *)``    |
    +-----------------------------------------+---------------------------+
    | ``RTEMS_CLOCK_GET_TICKS_SINCE_BOOT``    | ``(rtems_interval *)``    |
    +-----------------------------------------+---------------------------+
    |``RTEMS_CLOCK_GET_TICKS_PER_SECOND``     | ``(rtems_interval *)``    |
    +-----------------------------------------+---------------------------+
    | ``RTEMS_CLOCK_GET_TIME_VALUE``          | ``(struct timeval *)``    |
    +-----------------------------------------+---------------------------+

NOTES:
    This directive is callable from an ISR.

    This directive will not cause the running task to be preempted.
    Re-initializing RTEMS causes the system date and time to be reset to an
    uninitialized state.  Another call to ``rtems_clock_set`` is required to
    re-initialize the system date and time to application specific
    specifications.
