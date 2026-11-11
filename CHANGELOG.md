# Changelog

## [Unreleased]

## [1.0.0] - 2026-11-11

### Added

- **`WarpDatePicker`**, a drop-in for Filament's `DatePicker` that knows public holidays of 49 countries and 157 calendars with their states and regions (Yasumi; Japan from the Cabinet Office list), shows their names on the calendar, and shows 1–12 months at once or `inline()`. Dates can be typed in the order of each language, and the calendar works with the keyboard.
- **Company days off** in code (`closures()`), on the **Company calendar** page (per tenant, with yearly repeats, half days and working days such as a Saturday you work), or from **iCal links** that keep their days when a link fails. Events with a time close only those hours, and events that can't be read for sure are left out and counted. A signed link to subscribe to the company days off in a calendar app, and *Make a new link* to stop a shared one.
- **Days off in the field:** `onDayOff()` to allow, warn, suggest the next business day or refuse them (the `BusinessDay` rule), `minBusinessDaysFromToday()`, `showBusinessDaysFromToday()`, `workingDates()`, `dayLabels()` and `unavailableDates()` for your own data on the calendar.
- **`WarpDateRangePicker`** with presets (fiscal year from `fiscalYearStartMonth()`), `maxDays()`, `fields()` for two columns and the *First day* / *Last day* buttons, and **`WarpDateRangeFilter`** with *business days only* / *days off only*.
- **Dates that follow another field:** `PaymentTerms` (net days, business days, end of month, monthly closing and payment days) with `Roll`, and `TimeTerms`, for `fillFrom()` and `minBusinessDaysAfter()`.
- **`WarpDateColumn` and `WarpDateEntry`** with business days left or late, also in Warp Table's `->warp()` tables.
- **Business hours:** `businessHours()` per weekday, `breaks()`, `hoursBeforeHolidays()`, `hoursOn()`, `closedHours()`, `cutoff()` and `timezone()`, on the panel, per place or painted on the Company calendar page; an *Open now* badge in the top bar and a signed JSON link with the next business time.
- **`WarpDateTimePicker`**, a drop-in for `DateTimePicker`, with times as buttons, lists or a clock face, typed times in many languages, `businessHoursOnly()`, `minBusinessHoursFromNow()`, `showBusinessHoursFromNow()`, `slotCounts()`, `timeLabels()`, `unavailableTimes()` and `alsoShowTimezones()`; times that don't exist or come twice on a clock-change day are refused. **`WarpDateTimeRangePicker`** with lengths (also in business hours) and overnight ranges. **`WarpDateTimeColumn`**, **`WarpDateTimeEntry`** and **`BusinessHoursFilter`**.
- **A calendar per place** with `calendars()` (a warehouse or a shop with its own country, working days, hours and time zone), also built from each tenant's records with a closure.
- **Business days and hours in PHP:** `HolidayCalendar` (`isBusinessDay()`, `addBusinessDays()`, `businessDaysBetween()`, `addBusinessHours()`, `nextOpen()`, `isOpen()` and more) and Carbon macros such as `now()->addBusinessDays(3)`.
- **Filament multi-tenancy:** days off, business hours and links are kept per tenant; `WarpDate::get()->asTenant($team, fn () => …)` reads one team's calendar from commands, queued jobs or a panel without tenancy, and `php artisan warp:assign-tenant` gives existing rows a tenant.
- **Who can change days off:** `canManageClosures()` or the `warp-datepicker.manage-closures` gate. iCal links to the server's own network are refused, and the links that open without signing in are rate limited.
- Instructions for AI coding assistants through [Laravel Boost](https://laravel.com/docs/boost) (a guideline and the `warp-datepicker-development` skill), and ten languages.
