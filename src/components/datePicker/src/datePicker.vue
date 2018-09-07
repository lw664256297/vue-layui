<template>
	<div>
		<input class="layui-input" @click.stop="show=!show" :value="current | dateFormat" type="text" readonly>

		<div id="layui-laydate1" class="layui-laydate" v-show="show">
			<div class="layui-laydate-main laydate-main-list-0">
				<div class="layui-laydate-header" @click.stop="">
					<!-- 切换年份 -->
					<i class="layui-icon layui-icon-prev laydate-prev-y" @click.stop="selectYear=!selectYear"></i>
					<!-- 切换月份 -->
					<i class="layui-icon layui-icon-left laydate-prev-m" @click.stop="switchMonth(-1)"></i>

					<div class="laydate-set-ym">
						<span>{{select.year}}年</span>
						<span>{{select.month}}月</span>
					</div>
					<i class="layui-icon layui-icon-right" @click.stop="switchMonth(1)"></i>
					<i class="layui-icon layui-icon-next laydate-next-y"></i>
				</div>
				<div class="layui-laydate-content">
					<table>
						<thead>
							<tr>
								<th>日</th>
								<th>一</th>
								<th>二</th>
								<th>三</th>
								<th>四</th>
								<th>五</th>
								<th>六</th>
							</tr>
						</thead>
						<tbody>
							<tr v-for="week of list">
								<td v-for="weekday of week" @click="pick(weekday)"
	                                :class="{
	                                    'flag': weekday.flag, 
	                                    'layui-this': !weekday.flag && weekday.text == current.date
	                                         && select.month == current.month && select.year == current.year}">
	                                {{weekday.text}}
                            	</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>
			<div class="layui-laydate-footer">
				<div class="laydate-footer-btns">
					<span lay-type="clear" class="laydate-btns-clear">清空</span>
					<span lay-type="now" class="laydate-btns-now">现在</span>
					<span lay-type="confirm" class="laydate-btns-confirm">确定</span>
				</div>
			</div>
		</div>
	</div>

</template>
<script>
    export default {
    	name:"lay-date-Picker",
        props: {
        	format: String,
            moment: {
                type: Number,
                default() {
                    return new Date().getTime()
                }
            }
        },
        data() {
            return {
                show: false,    // 控制日历面板的显示与隐藏
                selectYear: false,  // 控制年份的面板的显示和隐藏
                current: '',    // 已选择的日期时间。yyyy-MM-dd
                select: {       // 已选择的日期对象
                    year: '',
                    month: '',
                    date: '',
                    day: ''
                },
                currentMonthFirstDay: null, // 当前月的1号属于星期几
                currentMonthEndDate: null,  // 当前月的最后一天是几号
                currentMonthEndDay: null,   // 当前月的最后一天属于星期几
                lastMonthEndDate: null,     // 上个月的最后一天是几号
                list: [],   // 日历的二维数组
                years: [],  // 1900-2100
                months: ['一月','二月','三月','四月','五月','六月','七月','八月','九月','十月','十一月','十二月']
            }
        },
        mounted(){
        	console.log(this.format);
        },
        watch: {
            select: {
                handler(newVal) {
                    let pre
                    if (newVal.month == 1) {
                        pre = new Date(newVal.year - 1, 12, 0)
                    } else {
                        pre = new Date(newVal.year, newVal.month - 1, 0)
                    }
                    this.lastMonthEndDate = pre.getDate()
                    // 获取日历排表
                    this.getDateList()    
                },
                deep: true
            },
            show(newVal) {
            	console.log(newVal);
                if (newVal) {
                    document.addEventListener('click', this.bindEvent)
                } else {
                    document.removeEventListener('click', this.bindEvent)
                }
            },
            // 打开年份选择器的时候使当前年份、月份出现在窗口顶部
            selectYear(newVal) {
                if (newVal) {
                    this.$nextTick(() => {
                        const year = this.$refs.year
                        const month = this.$refs.month
                        const y = year.getElementsByClassName('active')[0].innerHTML
                        const m = month.getElementsByClassName('active')[0].innerHTML
                        year.scrollTop = (y - 1900) * 30
                        month.scrollTop = (this.select.month - 1) * 30
                    })
                }
            }
        },
        created() {
            this.transform(this.moment)
            this.complete()
            // 获得年份列表： 1900-2100
            for(let i = 1900; i <= 2100; i++) {
                this.years.push(i)
            }
        },
        filters: {
            // 日期格式过滤器
            dateFormat(val) {
                 if (!val) {
                     return ''
                 }
                 return `${val.year}-${val.month}-${val.date}`.replace(/\d+/g, (a) => {
                     return (a.length === 4) ? a : ((a.length === 2) ? a : ('0' + a))
                 })
            }
        },
        methods: {
            /**
            * 将时间转化为具体的 年、月、日、星期
            **/
            transform(time) {
                const date = new Date(time)
                this.select.year = date.getFullYear()
                this.select.month = date.getMonth() + 1
                this.select.date = date.getDate()
                this.select.day = date.getDay()
                this.currentMonthFirstDay = 
                    new Date(this.select.year, this.select.month - 1, 1, 0).getDay()
                this.currentMonthEndDate = 
                    new Date(this.select.year, this.select.month, 0).getDate()
                this.currentMonthEndDay =
                    new Date(this.select.year, this.select.month, 0).getDay()
            },
            /*
            * 计算出日历列表，二维数组
            * 第一层为星期，第二层为每星期的第几天
            */
            getDateList() {
                this.list = []
                // 获取日历第一行的数据（需加上第一个星期中所包含上个月的几天）
                let temp = this.lastMonthEndDate - (this.currentMonthFirstDay - 1)
                let list = 
                    this.numberList(temp, this.lastMonthEndDate, true)
                    .concat(this.numberList(1, 7 - this.currentMonthFirstDay))
                this.list.push(list)
                temp = (7 - this.currentMonthFirstDay) + 1
                
                /*
                * 剩下的行数
                */
                // 计算除了第一行剩下的天数
                const leftDays = this.currentMonthEndDate - (7 - this.currentMonthFirstDay)
                // 剩下的星期数
                const lineNumber = Math.ceil(leftDays / 7)
                // 包含下个月日历的天数
                const nextDays = 7 - (leftDays % 7)
                for (let i = 0; i < lineNumber; i++) {
                    if (i === lineNumber - 1 && nextDays > 0 && nextDays !== 7) {
                        this.list[lineNumber] = 
                            this.numberList(temp, this.currentMonthEndDate)
                            .concat(this.numberList(1, nextDays, true))
                    } else {
                        this.list.push(this.numberList(temp, temp + 6))
                    }
                    temp = temp + 7
                }
            },
            /*
            * 获得日历数组
            * start: 开始日
            * end: 结束日
            * flag： boolean值，判断是否属于本月
            */
            numberList(start, end, flag) {
                let list = []
                for (let i = start; i <= end; i++) {
                    list.push({
                        text: i,
                        flag: !!flag
                    })
                }
                return list
            },
            /*
            * 切换月份， -1为当前月份-1，+1为当前月份+1
            */
            switchMonth(n) {
                let year = this.select.year
                if (n===-1) {
                    const pre = this.select.month === 1 ? 12 : this.select.month - 1
                    if (pre === 12) {
                        this.transform(new Date(year - 1, pre - 1, this.select.date))
                    } else {
                        this.transform(new Date(year, pre - 1, this.select.date))
                    }
                } else {
                    const next = this.select.month === 12 ? 1 : this.select.month + 1
                    if (next === 1) {
                        this.transform(new Date(year + 1, next - 1, this.select.date))
                    } else {
                        this.transform(new Date(year, next - 1, this.select.date))
                    }
                }
            },
            pick(day) {
                if (!!day.flag) {
                    // 当页日历上可能还会显示部分上个月或者下个月的部分天数，根据标识来做判断
                    // 并对月份作出相应的处理
                    if (parseInt(day.text) > 15) {
                        this.transform(new Date(this.select.year, this.select.month - 2, parseInt(day.text)))
                    } else {
                        this.transform(new Date(this.select.year, this.select.month, parseInt(day.text)))
                    }
                } else {
                    this.transform(new Date(this.select.year, this.select.month - 1, parseInt(day.text)))
                }
                this.complete()
            },
            // 绑定事件：点击关闭日历面板
            bindEvent() {
                this.show = false
                this.selectYear = false
            },
            // 选取年
            pickYear(n) {
                this.transform(new Date(n, this.select.month - 1, this.select.date))
                this.complete()
            },
            // 选取月
            pickMonth(n) {
                this.transform(new Date(this.select.year, n - 1, this.select.date))
                this.complete()
            },
            // 更改选中时间并向父组件派发事件
            complete() {
                // 触发父组件的传过来的picked事件。三个参数: 年，月，日
                this.$emit('picked', this.select.year, this.select.month, this.select.date)
                this.current = {
                    year: this.select.year,
                    month: this.select.month,
                    date: this.select.date
                }
            }
        }
    }
</script>

<style scoped>
	/** layui-v2.4.3 MIT License By https://www.layui.com */
	.laydate-set-ym,
	.layui-laydate,
	.layui-laydate *,
	.layui-laydate-list {
		box-sizing: border-box
	}
	
	html #layuicss-laydate {
		display: none;
		position: absolute;
		width: 1989px
	}
	
	.layui-laydate * {
		margin: 0;
		padding: 0
	}
	
	.layui-laydate {
		position: absolute;
		z-index: 66666666;
		margin: 5px 0;
		border-radius: 2px;
		font-size: 14px;
		-webkit-animation-duration: .3s;
		animation-duration: .3s;
		-webkit-animation-fill-mode: both;
		animation-fill-mode: both;
		-webkit-animation-name: laydate-upbit;
		animation-name: laydate-upbit
	}
	
	.layui-laydate-main {
		width: 272px
	}
	
	.layui-laydate-content td,
	.layui-laydate-header *,
	.layui-laydate-list li {
		transition-duration: .3s;
		-webkit-transition-duration: .3s
	}
	
	@-webkit-keyframes laydate-upbit {
		from {
			-webkit-transform: translate3d(0, 20px, 0);
			opacity: .3
		}
		to {
			-webkit-transform: translate3d(0, 0, 0);
			opacity: 1
		}
	}
	
	@keyframes laydate-upbit {
		from {
			transform: translate3d(0, 20px, 0);
			opacity: .3
		}
		to {
			transform: translate3d(0, 0, 0);
			opacity: 1
		}
	}
	
	.layui-laydate-static {
		position: relative;
		z-index: 0;
		display: inline-block;
		margin: 0;
		-webkit-animation: none;
		animation: none
	}
	
	.laydate-ym-show .laydate-next-m,
	.laydate-ym-show .laydate-prev-m {
		display: none!important
	}
	
	.laydate-ym-show .laydate-next-y,
	.laydate-ym-show .laydate-prev-y {
		display: inline-block!important
	}
	
	.laydate-time-show .laydate-set-ym span[lay-type=month],
	.laydate-time-show .laydate-set-ym span[lay-type=year],
	.laydate-time-show .layui-laydate-header .layui-icon,
	.laydate-ym-show .laydate-set-ym span[lay-type=month] {
		display: none!important
	}
	
	.layui-laydate-header {
		position: relative;
		line-height: 30px;
		padding: 10px 70px 5px
	}
	
	.laydate-set-ym span,
	.layui-laydate-header i {
		padding: 0 5px;
		cursor: pointer
	}
	
	.layui-laydate-header * {
		display: inline-block;
		vertical-align: bottom
	}
	
	.layui-laydate-header i {
		position: absolute;
		top: 10px;
		color: #999;
		font-size: 18px
	}
	
	.layui-laydate-header i.laydate-prev-y {
		left: 15px
	}
	
	.layui-laydate-header i.laydate-prev-m {
		left: 45px
	}
	
	.layui-laydate-header i.laydate-next-y {
		right: 15px
	}
	
	.layui-laydate-header i.laydate-next-m {
		right: 45px
	}
	
	.laydate-set-ym {
		width: 100%;
		text-align: center;
		text-overflow: ellipsis;
		overflow: hidden;
		white-space: nowrap
	}
	
	.laydate-time-text {
		cursor: default!important
	}
	
	.layui-laydate-content {
		position: relative;
		padding: 10px;
		-moz-user-select: none;
		-webkit-user-select: none;
		-ms-user-select: none
	}
	
	.layui-laydate-content table {
		border-collapse: collapse;
		border-spacing: 0
	}
	
	.layui-laydate-content td,
	.layui-laydate-content th {
		width: 36px;
		height: 30px;
		padding: 5px;
		text-align: center
	}
	
	.layui-laydate-content td {
		position: relative;
		cursor: pointer
	}
	
	.laydate-day-mark {
		position: absolute;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		line-height: 30px;
		font-size: 12px;
		overflow: hidden
	}
	
	.laydate-day-mark::after {
		position: absolute;
		content: '';
		right: 2px;
		top: 2px;
		width: 5px;
		height: 5px;
		border-radius: 50%
	}
	
	.layui-laydate-footer {
		position: relative;
		height: 46px;
		line-height: 26px;
		padding: 10px 20px
	}
	
	.layui-laydate-footer span {
		margin-right: 15px;
		display: inline-block;
		cursor: pointer;
		font-size: 12px
	}
	
	.layui-laydate-footer span:hover {
		color: #5FB878
	}
	
	.laydate-footer-btns {
		position: absolute;
		right: 10px;
		top: 10px
	}
	
	.laydate-footer-btns span {
		height: 26px;
		line-height: 26px;
		margin: 0 0 0 -1px;
		padding: 0 10px;
		border: 1px solid #C9C9C9;
		background-color: #fff;
		white-space: nowrap;
		vertical-align: top;
		border-radius: 2px
	}
	
	.layui-laydate-list>li,
	.layui-laydate-range .layui-laydate-main {
		display: inline-block;
		vertical-align: middle
	}
	
	.layui-laydate-list {
		position: absolute;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		padding: 10px;
		background-color: #fff
	}
	
	.layui-laydate-list>li {
		position: relative;
		width: 33.3%;
		height: 36px;
		line-height: 36px;
		margin: 3px 0;
		text-align: center;
		cursor: pointer
	}
	
	.laydate-month-list>li {
		width: 25%;
		margin: 17px 0
	}
	
	.laydate-time-list>li {
		height: 100%;
		margin: 0;
		line-height: normal;
		cursor: default
	}
	
	.laydate-time-list p {
		position: relative;
		top: -4px;
		line-height: 29px
	}
	
	.laydate-time-list ol {
		height: 181px;
		overflow: hidden
	}
	
	.laydate-time-list>li:hover ol {
		overflow-y: auto
	}
	
	.laydate-time-list ol li {
		width: 130%;
		padding-left: 33px;
		line-height: 30px;
		text-align: left;
		cursor: pointer
	}
	
	.layui-laydate-hint {
		position: absolute;
		top: 115px;
		left: 50%;
		width: 250px;
		margin-left: -125px;
		line-height: 20px;
		padding: 15px;
		text-align: center;
		font-size: 12px
	}
	
	.layui-laydate-range {
		width: 546px
	}
	
	.layui-laydate-range .laydate-main-list-0 .laydate-next-m,
	.layui-laydate-range .laydate-main-list-0 .laydate-next-y,
	.layui-laydate-range .laydate-main-list-1 .laydate-prev-m,
	.layui-laydate-range .laydate-main-list-1 .laydate-prev-y {
		display: none
	}
	
	.layui-laydate-range .laydate-main-list-1 .layui-laydate-content {
		border-left: 1px solid #e2e2e2
	}
	
	.layui-laydate,
	.layui-laydate-hint {
		border: 1px solid #d2d2d2;
		box-shadow: 0 2px 4px rgba(0, 0, 0, .12);
		background-color: #fff;
		color: #666
	}
	
	.layui-laydate-header {
		border-bottom: 1px solid #e2e2e2
	}
	
	.layui-laydate-header i:hover,
	.layui-laydate-header span:hover {
		color: #5FB878
	}
	
	.layui-laydate-content {
		border-top: none 0;
		border-bottom: none 0
	}
	
	.layui-laydate-content th {
		font-weight: 400;
		color: #333
	}
	
	.layui-laydate-content td {
		color: #666
	}
	
	.layui-laydate-content td.laydate-selected {
		background-color: #00F7DE
	}
	
	.laydate-selected:hover {
		background-color: #00F7DE!important
	}
	
	.layui-laydate-content td:hover,
	.layui-laydate-list li:hover {
		background-color: #eaeaea;
		color: #333
	}
	
	.laydate-time-list li ol {
		margin: 0;
		padding: 0;
		border: 1px solid #e2e2e2;
		border-left-width: 0
	}
	
	.laydate-time-list li:first-child ol {
		border-left-width: 1px
	}
	
	.laydate-time-list>li:hover {
		background: 0 0
	}
	
	.layui-laydate-content .laydate-day-next,
	.layui-laydate-content .laydate-day-prev {
		color: #d2d2d2
	}
	
	.laydate-selected.laydate-day-next,
	.laydate-selected.laydate-day-prev {
		background-color: #f8f8f8!important
	}
	
	.layui-laydate-footer {
		border-top: 1px solid #e2e2e2
	}
	
	.layui-laydate-hint {
		color: #FF5722
	}
	
	.laydate-day-mark::after {
		background-color: #5FB878
	}
	
	.layui-laydate-content td.layui-this .laydate-day-mark::after {
		display: none
	}
	
	.layui-laydate-footer span[lay-type=date] {
		color: #5FB878
	}
	
	.layui-laydate .layui-this {
		background-color: #009688!important;
		color: #fff!important
	}
	
	.layui-laydate .laydate-disabled,
	.layui-laydate .laydate-disabled:hover {
		background: 0 0!important;
		color: #d2d2d2!important;
		cursor: not-allowed!important;
		-moz-user-select: none;
		-webkit-user-select: none;
		-ms-user-select: none
	}
	
	.laydate-theme-molv {
		border: none
	}
	
	.laydate-theme-molv.layui-laydate-range {
		width: 548px
	}
	
	.laydate-theme-molv .layui-laydate-main {
		width: 274px
	}
	
	.laydate-theme-molv .layui-laydate-header {
		border: none;
		background-color: #009688
	}
	
	.laydate-theme-molv .layui-laydate-header i,
	.laydate-theme-molv .layui-laydate-header span {
		color: #f6f6f6
	}
	
	.laydate-theme-molv .layui-laydate-header i:hover,
	.laydate-theme-molv .layui-laydate-header span:hover {
		color: #fff
	}
	
	.laydate-theme-molv .layui-laydate-content {
		border: 1px solid #e2e2e2;
		border-top: none;
		border-bottom: none
	}
	
	.laydate-theme-molv .laydate-main-list-1 .layui-laydate-content {
		border-left: none
	}
	
	.laydate-theme-grid .laydate-month-list>li,
	.laydate-theme-grid .laydate-year-list>li,
	.laydate-theme-grid .layui-laydate-content td,
	.laydate-theme-grid .layui-laydate-content thead,
	.laydate-theme-molv .layui-laydate-footer {
		border: 1px solid #e2e2e2
	}
	
	.laydate-theme-grid .laydate-selected,
	.laydate-theme-grid .laydate-selected:hover {
		background-color: #f2f2f2!important;
		color: #009688!important
	}
	
	.laydate-theme-grid .laydate-selected.laydate-day-next,
	.laydate-theme-grid .laydate-selected.laydate-day-prev {
		color: #d2d2d2!important
	}
	
	.laydate-theme-grid .laydate-month-list,
	.laydate-theme-grid .laydate-year-list {
		margin: 1px 0 0 1px
	}
	
	.laydate-theme-grid .laydate-month-list>li,
	.laydate-theme-grid .laydate-year-list>li {
		margin: 0 -1px -1px 0
	}
	
	.laydate-theme-grid .laydate-year-list>li {
		height: 43px;
		line-height: 43px
	}
	
	.laydate-theme-grid .laydate-month-list>li {
		height: 71px;
		line-height: 71px
	}
	.flag{
		color:#d2d2d2!important;
	}
</style>