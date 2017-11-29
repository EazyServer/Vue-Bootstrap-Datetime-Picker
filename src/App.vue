<template>
    <div>
        <table style="width: 100%">
            <tr>
                <td>
                    <datetime-picker v-model="date1"
                                     :first-day="1"
                                     :show-dst="false"
                                     :show-hours="true"
                                     :show-minutes="true"
                                     :show-seconds="true"
                    ></datetime-picker>
                    <p>Date 1 is: <b>{{ date1 }}</b></p>
                    <div class="well"><pre><small>v-model="date1"
:first-day="1"
:show-dst="false"
:show-hours="true"
:show-minutes="true"
:show-seconds="true"</small></pre>
                    </div>

                </td>
                <td>
                    <datetime-picker v-model="date2"
                                     :first-day="0"
                                     :show-dst="true"
                    ></datetime-picker>
                    <p>Date 2 is: <b>{{ date1 }}</b></p>
                    <div class="well"><pre><small>v-model="date2"
:first-day="2"
:show-dst="true"



</small></pre>
                    </div>

                </td>
            </tr>

            <tr>
                <td>
                    <datetime-picker v-model="date3"
                                     :first-day="1"
                                     :show-hours="true"
                                     :hour-step="2"
                                     :show-minutes="true"
                                     :minute-step="10"
                                     :show-seconds="true"
                                     :second-step="30"
                    ></datetime-picker>
                    <p>Date 3 is: <b>{{ date3 }}</b></p>
                    <div class="well"><pre><small>v-model="date3"
:first-day="1"
:show-hours="true"
:hour-step="2"
:show-minutes="true"
:minute-step="10"
:show-seconds="true"
:second-step="30"</small></pre>
                    </div>

                </td>
                <td>
                    <datetime-picker v-model="date4"
                                     :first-day="0"
                                     :show-dST="true"
                                     :show-hours="true"
                                     :hour-step="2"
                                     :show-minutes="true"
                                     :minute-step="5"
                    ></datetime-picker>
                    <p>Date 3 is: <b>{{ date3 }}</b></p>
                    <div class="well"><pre><small>v-model="date4"
:first-day="0"
:show-dst="true"
:show-hours="true"
:hour-step="2"
:show-minutes="true"
:minute-step="5"

</small></pre>
                    </div>

                </td>
            </tr>
        </table>
    </div>
</template>

<script>
	import moment from "moment";
	import {DatetimePicker} from './main';

	export default {
		data() {
			return {
				date1: moment().locale( 'en' ).format( 'YYYY-MM-DDTHH:mm:ss' ),
				date2: moment().locale( 'en' ).format( 'YYYY-MM-DDTHH:mm:ss' ),
				date3: moment().locale( 'en' ).format( 'YYYY-MM-DDTHH:mm:ss' ),
				date4: moment().locale( 'en' ).format( 'YYYY-MM-DDTHH:mm:ss' ),
			};
		},
		components: {
			DatetimePicker
		}
	};
</script>