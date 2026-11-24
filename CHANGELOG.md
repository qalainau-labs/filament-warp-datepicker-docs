# Changelog

## [Unreleased]

## [1.2.2] - 2026-11-25

### Fixed

- **A date typed before the picker's script finished loading was lost**: the picker loads its script when it appears, and on a slow connection or a busy device a date typed right away was cleared when the script arrived, so the field stayed empty and the record could be saved without it. The typed text now stays in the field and becomes the picker's value (when you have already moved to another field, it is read just as if you had pressed Enter). Nothing to change in your app.

## [1.2.1] - 2026-11-25

### Fixed

- **`WarpPeriodFilter` and `WarpDateRangeFilter` can now use an index on the date column.** They compared `DATE(column)` (`whereDate()`), which stops MySQL, MariaDB and PostgreSQL from using an index on that column, so filtering a large table by month or range read far more rows than needed. They now compare the column itself (`>= first day` and `< the day after the last day`). The rows you get are the same for date and datetime columns, including the last day's late times and the "Business days only" / "Days off only" choices.

## [1.2.0] - 2026-11-23

### Added

- **Time-only field `WarpTimePicker`**: pick just a time (saved as `H:i`, like Filament's `TimePicker`) with the same lists, typing and gray business hours as `WarpDateTimePicker`. `businessHours()` grays the closed times, `businessHoursOnly()` refuses them (also on the server), and `forDay()` / `forDateField()` choose whose day's hours apply. See [Time only](README.md#time-only).
- **Several separate days: `WarpDatePicker::multiple()`**: click days one by one; they show as chips you can remove, and are saved as a sorted list of `Y-m-d` dates. `minItems()` / `maxItems()` and the days-off rules are checked on the server for every day. Off unless you call it; the field then needs an `array` cast. See [Several separate days](README.md#several-separate-days).
- **Month, week, quarter or year: `WarpPeriodPicker` and `WarpPeriodFilter`**: one click picks “November 2026”, ISO week 46 or Q3. Saves the first day of the period (or a key like `2026-11`, `2026-W46`, `2026-Q3` with `storeAsKey()`), can fill the last day into another field, and follows your fiscal year with `fiscalYearStartsIn(4)`. See [Month, week, quarter or year](README.md#month-week-quarter-or-year).
- **Public holidays for China, Taiwan, Hong Kong, Singapore and India**: these countries were not available before (choosing them threw an error). Their holidays, including lunar-calendar ones, ship with the plugin up to 2030, and the weekend working days China and Taiwan swap in count as business days. See [Countries, states and working days](README.md#countries-states-and-working-days).
- **Japanese era and other calendars: `displayCalendar()`**: show and type dates as 令和8年11月6日 / `R8.11.6`, 民國115年11月6日 or the Thai Buddhist year, on date fields, columns and entries. Dates are still saved as normal `Y-m-d`. Off unless you call it. See [Japanese era and other calendars](README.md#japanese-era-and-other-calendars).
- **Week numbers: `weekNumbers()` and `isoWeek()`**: ISO week numbers beside the calendar, and `2026-W45` in columns and entries. See [Week numbers](README.md#week-numbers).

### Fixed

- **Typing displayed dates**: dates with localized month and weekday names, including punctuation and custom `displayFormat()` values, can now be typed back into the field in all ten supported languages. Numeric shortcuts still work; a mismatched weekday does not change the date.

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
