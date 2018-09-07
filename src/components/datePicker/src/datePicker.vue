<template>
	<div>
		<input type="text" class="layui-input">
		<div id="layui-laydate1" class="layui-laydate">
			<div class="layui-laydate-main laydate-main-list-0">
				<div class="layui-laydate-header">
					<i class="layui-icon layui-icon-prev laydate-prev-y"></i>
					<i class="layui-icon layui-icon-left laydate-prev-m"></i>
					<div class="laydate-set-ym">
						<span lay-ym="2018-9" lay-type="year">2018年</span>
						<span lay-ym="2018-9" lay-type="month">9月</span>
					</div>
					<i class="layui-icon layui-icon-right"></i>
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
							<tr>
								<td class="laydate-day-prev" lay-ymd="2018-8-26">26</td>
								<td class="laydate-day-prev" lay-ymd="2018-8-27">27</td>
								<td class="laydate-day-prev" lay-ymd="2018-8-28">28</td>
								<td class="laydate-day-prev" lay-ymd="2018-8-29">29</td>
								<td class="laydate-day-prev" lay-ymd="2018-8-30">30</td>
								<td class="laydate-day-prev" lay-ymd="2018-8-31">31</td>
								<td lay-ymd="2018-9-1" class="">1</td>
							</tr>
							<tr>
								<td lay-ymd="2018-9-2" class="">2</td>
								<td lay-ymd="2018-9-3" class="">3</td>
								<td lay-ymd="2018-9-4" class="">4</td>
								<td lay-ymd="2018-9-5" class="">5</td>
								<td lay-ymd="2018-9-6" class="">6</td>
								<td class="layui-this" lay-ymd="2018-9-7">7</td>
								<td lay-ymd="2018-9-8" class="">8</td>
							</tr>
							<tr>
								<td lay-ymd="2018-9-9" class="">9</td>
								<td lay-ymd="2018-9-10" class="">10</td>
								<td lay-ymd="2018-9-11" class="">11</td>
								<td lay-ymd="2018-9-12" class="">12</td>
								<td lay-ymd="2018-9-13" class="">13</td>
								<td lay-ymd="2018-9-14" class="">14</td>
								<td lay-ymd="2018-9-15" class="">15</td>
							</tr>
							<tr>
								<td lay-ymd="2018-9-16" class="">16</td>
								<td lay-ymd="2018-9-17" class="">17</td>
								<td lay-ymd="2018-9-18" class="">18</td>
								<td lay-ymd="2018-9-19" class="">19</td>
								<td lay-ymd="2018-9-20" class="">20</td>
								<td lay-ymd="2018-9-21" class="">21</td>
								<td lay-ymd="2018-9-22" class="">22</td>
							</tr>
							<tr>
								<td lay-ymd="2018-9-23" class="">23</td>
								<td lay-ymd="2018-9-24" class="">24</td>
								<td lay-ymd="2018-9-25" class="">25</td>
								<td lay-ymd="2018-9-26" class="">26</td>
								<td lay-ymd="2018-9-27" class="">27</td>
								<td lay-ymd="2018-9-28" class="">28</td>
								<td lay-ymd="2018-9-29" class="">29</td>
							</tr>
							<tr>
								<td lay-ymd="2018-9-30" class="">30</td>
								<td class="laydate-day-next" lay-ymd="2018-10-1">1</td>
								<td class="laydate-day-next" lay-ymd="2018-10-2">2</td>
								<td class="laydate-day-next" lay-ymd="2018-10-3">3</td>
								<td class="laydate-day-next" lay-ymd="2018-10-4">4</td>
								<td class="laydate-day-next" lay-ymd="2018-10-5">5</td>
								<td class="laydate-day-next" lay-ymd="2018-10-6">6</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>
			<div class="layui-laydate-footer">
				<div class="laydate-footer-btns"><span lay-type="clear" class="laydate-btns-clear">清空</span><span lay-type="now" class="laydate-btns-now">现在</span><span lay-type="confirm" class="laydate-btns-confirm">确定</span></div>
			</div>
		</div>
	</div>

</template>

<script>
	export default {
		name: "lay-date-Picker",
		data() {
			return {

			}
		},
		mounted() {
			//初始化layui
			let that = this;
			console.log(that);
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
</style>