# DatepickerWrapper 
This package is a wrapper around Vue2-Datepicker.
 
## Documentation
Visit: [https://mengxiong10.github.io/](https://github.com/mengxiong10/vue2-datepicker#readme)

## Features & characteristics:
* Single date
* Date Ranges
* Formatting

## Install

```bash
npm i @dcodegroup-au/vue-datepicker
```

### Configuration Options
The Vue2 datepicker supports the following formats: date|datetime|year|month|time|week.
We are currently only supporting date and date ranges with this wrapper.

The format value set requires that the value prop data passed in has the same format.
For date ranges an array of 2 correctly formatted date items is required.

```//example.vue.js
<datepicker-wrapper
  :value="dateData"
  :is-range="true"
></datepicker-wrapper>
```

```//example.blade.php
<datepicker-wrapper
  :value="{{ json_encode([Carbon\Carbon::now()->format('d/m/Y'), Carbon\Carbon::now()->addDays(7)->format('d/m/Y')]) }}"
  :is-range="true"
  :is-form-input="true" // required for form inputs
  name="expected_delivery" // required for form inputs
></datepicker-wrapper>
```