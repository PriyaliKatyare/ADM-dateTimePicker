# ADM-dateTimePicker  
![Version](https://img.shields.io/badge/npm-v1.0.0-brightgreen.svg)
&nbsp;
![AngularJs](https://img.shields.io/badge/Pure-AngularJs-red.svg)
&nbsp;
![License MIT](http://img.shields.io/badge/License-MIT-lightgrey.svg?style=flat)

*Pure AngularJs Gregorian and Jalali smart datetimepicker by [ADM | Amirkabir Data Miners](https://adm-co.net)*

### Tags  
`AngularJs` `Gregorian` `Jalali` `Date & Time picker` `Two calendar in one datePicker` `RangePicker` `Update range limitation automatilcy` `Disabling pattern` `Responsive` `UNIX` `SASS` `Compass` `Bootstrap`

---

### Demo  
See ADMdtp live [HERE](https://amirkabirdataminers.github.io/ADM-dateTimePicker).

---

### Implementation steps

#### Step 1: Install ADM-dateTimePicker
````javascript
npm install adm-dtp
````
#### Step 2: Include the files in your app
```html
<!doctype html>
<html ng-app="myApp">
    <head>
        <link rel="stylesheet" href="css/bootstrap.min.css" />
        <link rel="stylesheet" href="css/ADM-dateTimePicker.css" />
        <script src="js/angular.min.js"></script>
        <script src="js/ADM-dateTimePicker.min.js"></script>
        ...
    </head>
    <body>
        ...
    </body>
</html>
```
#### Step 3: Inject the ADM-dateTimePicker module
```javascript
var app = angular.module('myApp', ['ADM-dateTimePicker']);
```
#### Step 4: Add the adm-dtp directive to a DOM element
```html
<adm-dtp ng-model='date'></adm-dtp>
```
---
### Options
#### Set options for entire of app
```javascript
app.config(['ADMdtpProvider', function(ADMdtp) {
    ADMdtp.setOptions({
        calType: 'gregorian',
        format: 'YYYY/MM/DD HH:MM',
        ...
    });
}]);
```
#### Set options for each directive
```html
<!-- pass options from controller -->
<adm-dtp ng-model='date1' options='date1_options'></adm-dtp>
<!-- or write them inline -->
<adm-dtp ng-model='date2' options='{calType: "jalali", format: "YYYY/MM/DD", default: 1450197600000}'></adm-dtp>
```
#### Quick look
Name  |	Type  |	Default |	Description
------------- | ------------- | ------------- | -------------
default | Number, String, Date | -- | Initial date can be Number(UNIX), String or Date
calType | String | gregorian | gregorian & jalali are available
disabled | Array | -- | Disable specific days with format of String, Date and UNIX, or days with pattern of 'i+[NUM]' and '[NUM]d+[NUM]
smartDisabling | Boolean | true | Whether change Sunday from Gregorian calendar to Friday in Jalali calendar by switching calendar type or not
format | String | YYYY/MM/DD HH:MM | MM/DD/YYYY & MM/DD/YYYY HH:MM & YYYY/MM/DD & YYYY/MM/DD HH:MM are available
multiple | Boolean | true | Whether user can change calendar type or not
autoClose | Boolean | false | Closing ADMdtp on selecting a day
transition | Boolean | true | Transition on loading days
---
### Disabling days
#### Disable specific days
```html
<!-- it accept both unix and string date -->
<adm-dtp ng-model="date" options="disabled:['2016/1/20', 1453408200000]"></adm-dtp>
```
#### Disable with pattern
Currently two types of patterns are availble:
* Days in a week: `i+[NUM]`
  * `i` -> will disable all Sundays in Gregorain calendar or Saturdays in Jalali calendars
  * `i+6` -> will disable all Saturdays in Gregorain calendar or Fridays in Jalali calendars
  * ...
* Days in a month: `[NUM]d+[NUM]`
  * `d+1` -> will disable the second day of all months
  * `2d` -> will disable the even days of all months
  * `2d+1` -> will disable the odd days of all months
  * ...

##### Inverse disabling:
  putting **Exclamation mark (!)** at the begining of the pattern will inverse disabling pattern:
* `!i+6` -> just Saturdays in Gregorain calendar or Fridays in Jalali calendars are available
* `!2d+1` -> it's exactly like `2d`

##### Combine patterns:
  patterns of the same type can be combine with **Ampersand (&)**. 
  mention that `['2d+1', '7d']` and `['2d+1&7d']` are equal, but `['!2d+1', '!7d']` and `['!2d+1&7d']` are completely differents. 
  
##### Smart disabling:
`i` in Gregorian calendar will disable Sundays (weekend) that is equal to Fridays (weekend) in Jalali calendar.  
option `smartDisabling: true` change Sunday from Gregorian calendar to Friday in Jalali calendar by switching calendar type,  
but `smartDisabling: false` makes no different. 
```html
<adm-dtp ng-model='date' options="disabled:['2016/1/20', '!i&i+1', '15d+2']"></adm-dtp>
```
---
### Full data
Beside ngModel you can access to date full details throw `full-data` attribute.
```html
<adm-dtp ng-model="date" full-data="date_details"></adm-dtp>
```
`date_details` contains following parameters:
```javascript
{
    formated: "2015/12/15",
    gDate: 2015-12-15T16:40:00.000Z,
    //gDate is Date format of selected date in Gregorian calendar
    unix: 1450197600000,
    year: 2015,
    month: 12,
    day: 15,
    hour: 20,
    minute: 10,
    minDate: null,
    maxDate: null,
    calType: "gregorian",
    format: "YYYY/MM/DD"
}
```
---
### Smart range picker
#### Static limitation
```html
<!-- mindate & maxdate accept both unix and string date -->
<adm-dtp ng-model="date" options="{default:'2015/12/15'}" mindate="1449866902553" maxdate="'2015/12/18'"></adm-dtp>
```
#### Dynamic limitation
No need to destroy datepickers anymore!
```html
<adm-dtp ng-model="date1" full-data="date1_detail" maxdate="{{date2_detail.unix}}"></adm-dtp>
<adm-dtp ng-model="date2" full-data="date2_detail" mindate="{{date1_detail.unix}}"></adm-dtp>
<adm-dtp ng-model="date3" mindate="{{date1_detail.unix}}" maxdate="{{date2_detail.unix}}"></adm-dtp>
```
---
### Disabling ADMdtp
```html
<!-- disable permanently -->
<adm-dtp ng-model='date' disable='true'></adm-dtp>

<!-- disable dynamicly -->
<adm-dtp ng-model='date1' ></adm-dtp>
<adm-dtp ng-model='date2' disable='{{!date1}}'></adm-dtp>
```
---
### Events
#### Show / Hide
```html
<adm-dtp ng-model="date" on-open="open()"></adm-dtp>
<adm-dtp ng-model="date" on-close="close()"></adm-dtp>
```
#### Change / Select
```html
<!-- Note 1: you can access to the selected date full details in below functions -->
<!-- Note 2: functions names are optional but input name must be 'date' -->

<!-- event on any change occurs on input -->
<adm-dtp ng-model="date" on-change="change(date)"></adm-dtp>
<!-- event on selecting a day -->
<adm-dtp ng-model="date" on-datechange="dateChanged(date)"></adm-dtp>
<!-- event on changing the time -->
<adm-dtp ng-model="date" on-datechange="timeChanged(date)"></adm-dtp>
```
