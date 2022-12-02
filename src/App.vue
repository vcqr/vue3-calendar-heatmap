<template>
	<div>
		<h4>Unit Select</h4>
		<div>
			<input type="radio" id="one" value="Dings" v-model="picked"/>
			<label for="one">Dings</label>
			<br/>
			<input type="radio" id="two" value="Da" v-model="picked"/>
			<label for="two">Da</label>
			<br/>
			<span>Current Unit: {{ picked }}</span><br/>
			<br/>

			<input type="radio" id="horizontal" value="horizontal" v-model="orientation"/>
			<label for="one">Horizontal</label>
			<br/>
			<input type="radio" id="vertical" value="vertical" v-model="orientation"/>
			<label for="two">Vertical</label>
		</div>
		<br>
		<h4>None</h4>
		<calendar-heatmap :values="[]" :end-date="endDate" :style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }"
						  :vertical="orientation === 'vertical'" :no-data-text="false"/>
		<br>
		<h4>Some</h4>
		<calendar-heatmap
			:values="values"
			:end-date="endDate"
			:style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }"
			:vertical="orientation === 'vertical'"
			no-data-text="NOTHING"
		/>
		<br>
		<h4>Some (rounded corners)</h4>
		<calendar-heatmap :values="values" :end-date="endDate" :style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }" :round="2"
						  :vertical="orientation === 'vertical'"/>
		<br>
		<h4>Some (circles)</h4>
		<calendar-heatmap :values="values" :end-date="endDate" :style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }" :round="5"
						  :vertical="orientation === 'vertical'"/>
		<br>
		<h4>Locale</h4>
		<calendar-heatmap :values="values" :end-date="endDate" :style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }" :locale="{
			months: ['Jan', 'Feb', 'Mär', 'Apr', 'Mai', 'Jun', 'Jul', 'Aug', 'Sep', 'Okt', 'Nov', 'Dez'],
			days: ['So', 'Mo', 'Di', 'Mi', 'Do', 'Fr', 'Sa'],
			on: 'am',
			less: 'Weniger',
			more: 'Mehr'
		
		}" :vertical="orientation === 'vertical'"/>
		<br>
		<h4>Tooltip Unit</h4>
		<calendar-heatmap :values="values" :end-date="endDate" :style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }" :tooltip-unit="picked"
						  :vertical="orientation === 'vertical'"/>
		<br>
		<h4>TooltipFormatter</h4>
		<calendar-heatmap
			:values="values"
			:end-date="endDate"
			:style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }"
			:tooltip-formatter="(c, u) => c.count ? (c.count / 3600 / 1000) + ' ' + u : 'NÖX'"
			no-data-text="NIX"
			:tooltip-unit="picked"
			:vertical="orientation === 'vertical'"
		/>

		<br>
		<h4>NoDataFormatter</h4>
		<calendar-heatmap
			:values="values"
			:end-date="endDate"
			:style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }"
			:tooltip-formatter="(c, u) => c.count ? c.count + ' ' + u : 'NÖX'"
			:no-data-text="(c, u) => {
				return 'No Data: ' + dateFormat('YYYY-mm-dd', c.date)
			}"
			:vertical="orientation === 'vertical'"
			:locale="locale"
		/>

		<br>
		<h4>darkMode</h4>
		<calendar-heatmap
		    darkMode
			:values="values"
			:end-date="endDate"
			:style="{ 'max-width': orientation === 'vertical' ? '145px' : '675px' }"
			:tooltip-formatter="(c, u) => c.count ? c.count + ' ' + u : 'NÖX'"
			:no-data-text="(c, u) => {
				return 'No Data: ' + dateFormat('YYYY-mm-dd', c.date)
			}"
			:vertical="orientation === 'vertical'"
		/>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Vue3CalendarHeatmap from '@/components/CalendarHeatmap.vue';
import CalendarHeatmap from '@/components/CalendarHeatmap.vue';
import { data } from './data';
import { count } from 'console';

export default defineComponent({
	name: 'ServeDev',
	components: {
		CalendarHeatmap,
		Vue3CalendarHeatmap
	},
	data() {
		return {
			values: data,
			endDate: new Date('2021-08-01'),
			picked: 'Dings',
			orientation: 'horizontal',
			locale: {
				months: ['一月', '二月', '三月', '四月', '五月', '六月', '七月', '八月', '九月', '十月', '十一', '十二'],
				days: ['周日', '周一', '周二', '周三', '周四', '周五', '周六'],
				on: 'on',
				less: '少',
				more: '多'
			},
			dateFormat: function (fmt: string, date: Date) {
				let ret;
				const opt: any = {
					"Y+": date.getFullYear().toString(),        // 年
					"m+": (date.getMonth() + 1).toString(),     // 月
					"d+": date.getDate().toString(),            // 日
					"H+": date.getHours().toString(),           // 时
					"M+": date.getMinutes().toString(),         // 分
					"S+": date.getSeconds().toString()          // 秒
					// 有其他格式化字符需求可以继续添加，必须转化成字符串
				};
				for (let k in opt) {
					ret = new RegExp("(" + k + ")").exec(fmt);
					if (ret) {
						fmt = fmt.replace(ret[1], (ret[1].length == 1) ? (opt[k]) : (opt[k].padStart(ret[1].length, "0")))
					};
				};
				return fmt;
			}
		}
	}
});
</script>

<style>
body {
	font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
	font-size: 12px;
}
</style>
