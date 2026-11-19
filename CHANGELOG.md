# Changelog

## [Unreleased]

## [1.1.3] - 2026-11-20

### Fixed

- **Console error `warpDate is not defined`**: on pages where Livewire re-renders a date field inside another Alpine component, the browser console could show `warpDate is not defined` before the component's script had arrived. An empty component is now registered up front, and the real one replaces it as soon as it loads. No visible change.

## [1.1.2] - 2026-11-17

### Fixed

- **Time fields in Spanish with `hours12(true)`**: the field shows times as “1:30 p. m.” (the way Spanish writes it), but typing a time the same way was not understood. Times written “p. m.” / “a. m.” (with any kind of space) are now read.

## [1.1.1] - 2026-11-16

### Fixed

- **Static analysis**: Larastan no longer reports “Call to an undefined method” for the business-day methods on Carbon (`addBusinessDays()`, `isBusinessDay()` …), also when one is called on the result of another. The plugin ships `extension.neon` (loaded automatically with `phpstan/extension-installer`; otherwise add it to `includes`). See “Static analysis” in the README.

## [1.1.0] - 2026-11-15

### Added

- `canAccessUsing()` and `canWriteUsing()` on the plugin decide, from your app's roles, who can open the company calendar page and who can only look at it. See [Restricting pages per role](README.md#restricting-pages-per-role).

## [1.0.2] - 2026-11-14

### Fixed

- Accessibility: days that are not working days, the legend, closure dates and other grey text now meet 4.5:1 (they were too light, especially on white). In dark mode the text links of the calendar (Today, Clear) are lighter. The scrolling table of opening hours on the company calendar page can be reached with the keyboard.

## [1.0.1] - 2026-11-12

### Changed

- Now requires Filament 5.11 or later (was 5.0). Parts of the plugin did not work on older 5.x versions, which were never tested. The tests now also run on the lowest supported versions before each release.

### Fixed

- Company calendar links: a malformed address such as `http:///file` is now refused with the usual "not a valid URL" message instead of a server error.
- A date and time field near the right edge of the page (for example in the right column of a Repeater row) kept the calendar where it opened when the list of times appeared beside it, so the later times ended up off the screen and could not be clicked. The calendar now moves back inside the screen when its width changes.

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
