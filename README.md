# Vue Bootstrap Datetime Picker

> Uses the magical power of VueJS v2 and beauty of Twitter Bootstraps to create a powerful Datetime picker using Vue `v-model`, Finally!! See the [Demo site](https://eazyserver.github.io/Vue-Bootstrap-Datetime-Picker/).


![Demo Vue Bootstrap Datetime picker](https://raw.githubusercontent.com/EazyServer/Vue-Bootstrap-Datetime-Picker/master/src/assets/vue-bootstrap-datetime-picker.png)


This package only uses bootstrap ``CSS``, NO ``bootstrap.js`` or ``jquery.js`` is needed for this project. Its purely Vue2 implementation.

The `DatetimePicker` uses the powerful `v-model` ([see here](https://vuejs.org/v2/guide/forms.html#Text))  to read and set Datetime. So no events firing is needed, basically it's Plug & Play.

## Install vue-bootstrap-DatetimePicker #

You can install via npm
   
    $ npm install -S vue-bootstrap-datetime-picker


Import ``DatetimePicker`` from the package like so:
```javascript
import {DatetimePicker} from 'vue-bootstrap-datetime-picker';
```

## How to use vue-bootstrap-datetime-picker #

Include ```DatetimePicker``` in you Vue App ```components``` then use ```<datetime-picker v-model='date' :first-day="x" ...></datetime-picker>``` in your code. 

```DatetimePicker``` supports the following options:

```javascript
v-model='date'
:first-day = 1 // one of these values {0 = Sunday, 1,2,3,4,5, 6=Saturday}
:show-dst = {true or false} // Show a star (*) indicator on Daylight saving time days. If not used will be assumed false
:show-hours = {true or false} // if not used will be assumed false
:hour-step = 1 // any int number, if not used will be assumed 1
:show-minutes = {true or false} // if not used WILL be assumed false
:minute-step = 1 // any int number, if not used will be assumed 1
:show-seconds = {true or false} // if not used WILL be assumed false
:second-step = 1 // any int number, if not used will be assumed 1
```

You may need to include datetime picker helper CSS, can be found [here](https://raw.githubusercontent.com/EazyServer/Vue-Bootstrap-Datetime-Picker/master/src/assets/datetime-picker.css)

``DatetimePicker`` will listen to [i18n](https://github.com/kazupon/vue-i18n) internationalization plugin if attached to `window` object. Otherwise the package will use english.
####Example:

In your ``App.vue``:

```javascript
<template>
    <div id="app">
        <datetime-picker
                v-model="date1"
	             :first-day="1"
	             :show-dst="false"
	             :show-hours="true"
	             :show-minutes="true"
	             :show-seconds="true"
        ></datetime-picker>
        <p>Date 1 is: <b>{{ date1 }}</b></p>
    </div>
</template>

<script>
    import {DatetimePicker} from 'vue-bootstrap-Datetime-Picker';
    export default {
        name: 'app',
        data() {
            date1: moment().locale( 'en' ).format( 'YYYY-MM-DDTHH:mm:ss' )
        },
        components: {
            DatetimePicker
        },
    }
</script>
```
* Please note the the supplied datetime MUST be in this format: ``YYYY-MM-DDThh:mm:ss`` as per the example above. This requirement is enforced by HTML 5 e.g. ``<input type="datetime-local" value='2018-01-22T22:00:00'>``

## Copyright and License

[Vue-Bootstrap-DatetimePicker](https://github.com/EazyServer/Vue-Bootstrap-Datetime-Picker) was written by [Yarob Al-Taay](https://twitter.com/TheEpicVoyage) and is released under the 
[MIT License](LICENSE.md).

Copyright (c) 2017 Yarob Al-Taay