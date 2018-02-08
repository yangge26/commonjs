# commonjs
一些常用的js方法
```

/**
 * 产生不重复随机数字数组
 * @param {Object} n
 */
function getRandom(n) {
	var a = [];
	for(var i = 1; i <= n; i++) {
		a.push(i);
	}
	var num = [];
	for(var i = 0; i < n - 1; i++) {
		var ran = Math.floor(Math.random() * (n - i));
		var b = a.splice(ran, 1);
		num.push(b[0]);
	}
	num.push(a[0]);
	return num;
}
/**
 * 生成从0到指定值的数字数组
 * @param {Object} n
 */
function getNumberArr(n) {
	var numbersArray = [];
	for(var i = 0; numbersArray.push(i++) < n - 1;);
	return numbersArray;
}
/**
 * 生成随机的字母数字字符串
 * @param {Object} len
 */
function generateRandomAlphaNum(len) {
	var rdmString = "";
	for(; rdmString.length < len; rdmString += Math.random().toString(36).substr(2));
	return rdmString.substr(0, len);
}
/**
 * 简单打乱数组
 * @param {Object} arr
 */
function breakArr(arr) {
	return arr.sort(function() {
		return Math.random() - 0.5
	});
}
/**
 * 日期格式定制
 * @param {Object} d
 * @param {Object} format
 */
function dateFormat(date, format) {
	var args = {
		"M+": date.getMonth() + 1,
		"d+": date.getDate(),
		"h+": date.getHours(),
		"m+": date.getMinutes(),
		"s+": date.getSeconds(),
		"q+": Math.floor((date.getMonth() + 3) / 3),
		"S": date.getMilliseconds()
	};
	if(/(y+)/.test(format))
		format = format.replace(RegExp.$1, (date.getFullYear() + "").substr(4 - RegExp.$1.length));
	for(var i in args) {
		var n = args[i];
		if(new RegExp("(" + i + ")").test(format))
			format = format.replace(RegExp.$1, RegExp.$1.length == 1 ? n : ("00" + n).substr(("" + n).length));
	}
	return format;
}
/**
 * 时间差距格式化
 * @param {Object} d
 */
function timeGap(d) {
	var nMinute = 60 * 1000;
	var nDistance = 0;
	if(typeof d === 'string') {
		d = new Date(d);
	}
	nDistance = new Date() - d;
	if(nDistance < 24 * 60 * nMinute) {
		if(nDistance >= 60 * nMinute) {
			return Math.floor(nDistance / (60 * nMinute)) + '小时前';
		} else {
			if(nDistance >= nMinute) {
				return Math.floor(nDistance / nMinute) + '分钟前';
			} else {
				return '刚刚';
			}
		}
	} else {
		if(nDistance > 48 * 60 * nMinute) {
			if(nDistance <= 10 * 24 * 60 * nMinute) {
				return Math.floor(nDistance / (24 * 60 * nMinute)) + '天前';
			} else {
				return d.getFullYear() + "-" + d.getMonth() + "-" + d.getDate() + " " + d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds();
			}
		} else {
			return '昨天';
		}
	}
}

//获取某年某月的天数
function getMonthDays(year, month) {
	return new Date(year, month + 1, 0).getDate();
}
/**
 * m等于多个不大于n的数字相加的情况总数
 * @param {Object} m
 * @param {Object} n
 */
function mn(m, n) {
	if(m === 0 || m === 1 || n === 1) {
		return 1;
	}
	if(m < n) {
		return mn(m, m);
	}
	return mn(m, n - 1) + mn(m - n, n);
}

/**
 * 获取网页地址对象
 */
function getSearchObj() {
	var obj = {};
	var arr = location.search.substring(location.search.indexOf('?') + 1).split('&');
	for(var i = 0; i < arr.length; i++) {
		var str = arr[i];
		var index = str.indexOf('=');
		var key = str.substring(0, index);
		var val = str.substring(index + 1);
		obj[key] = val;
	}
	return obj;
}
//冒泡排序从小到大
function arrSort1(arr) {
	if(arr == undefined) {
		return false;
	}
	var len = arr.length;
	for(var i = 0; i < len; i++) {
		var b = true;
		for(var j = 0; j < len - i - 1; j++) {
			if(arr[j + 1] < arr[j]) {
				b = false;
				var temp = arr[j + 1];
				arr[j + 1] = arr[j];
				arr[j] = temp;
			}
		}
		if(b) {
			break;
		}
	}
}
//选择排序从小到大
function arrSort2(arr) {
	if(arr == undefined) {
		return false;
	}
	var len = arr.length;
	for(var i = 0; i < len; i++) {
		var index = i;
		for(var j = i + 1; j < len; j++) {
			if(arr[j] < arr[index]) {
				index = j;
			}
		}
		if(index != i) {
			var temp = arr[index];
			arr[index] = arr[i];
			arr[i] = temp;
		}
	}
}
//数组去重
function uniqueArr(arr) {
	var oTemp = {},
		aTemp = [];
	for(var i = 0; i < arr.length; i++) {
		var key = arr[i];
		if(!oTemp[key]) {
			oTemp[key] = true;
			aTemp.push(key);
		}
	}
	return aTemp;
}
//数字金额转中文金额
function digitUppercase(n) {
	var fraction = ['角', '分'];
	var digit = [
		'零', '壹', '贰', '叁', '肆',
		'伍', '陆', '柒', '捌', '玖'
	];
	var unit = [
		['元', '万', '亿'],
		['', '拾', '佰', '仟']
	];
	var head = n < 0 ? '欠' : '';
	n = Math.abs(n);
	var s = '';
	for(var i = 0; i < fraction.length; i++) {
		s += (digit[Math.floor(n * 10 * Math.pow(10, i)) % 10] + fraction[i]).replace(/零./, '');
	}
	s = s || '整';
	n = Math.floor(n);
	for(var i = 0; i < unit[0].length && n > 0; i++) {
		var p = '';
		for(var j = 0; j < unit[1].length && n > 0; j++) {
			p = digit[n % 10] + unit[1][j] + p;
			n = Math.floor(n / 10);
		}
		s = p.replace(/(零.)*零$/, '').replace(/^$/, '零') + unit[0][i] + s;
	}
	return head + s.replace(/(零.)*零元/, '元')
		.replace(/(零.)+/g, '零')
		.replace(/^整$/, '零元整');
}
//数字格式化
function numFormat(num, n) {
	var numStr = '' + num;
	var zeroStr = '';
	var len = numStr.length;

	for(var i = 0; i < n - len; i++) {
		zeroStr += '0';
	}
	return zeroStr + numStr;
}
//格式化
function format() {
	if(arguments.length == 0)
		return null;

	var result = arguments[0];
	for(var i = 1; i < arguments.length; i++) {
		var re = new RegExp('\\{' + (i - 1) + '\\}', 'gm');
		result = result.replace(re, arguments[i]);
	}
	return result;
}
//设置cookie
function setCookie(name, value, days) {
	var exp = new Date();
	exp.setTime(exp.getTime() + days * 24 * 60 * 60 * 1000);
	document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString();
}
//获取cookie
function getCookie(name) {
	var arr, reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
	if(arr = document.cookie.match(reg)) {
		return unescape(arr[2]);
	}
	return null;
}
//删除cookie
function delCookie(name) {
	var exp = new Date();
	exp.setTime(exp.getTime() - 1);
	var cval = getCookie(name);
	if(cval != null) {
		document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString();
	}
}
//验证是否为手机
function isPhone(phone) {
	var reg = /^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$/;
	return reg.test(phone);
}
//验证是否为日期
function isDate(dstr) {
	var reg = /^\d{4}-\d{1,2}-\d{1,2}$/;
	return reg.test(dstr);
}
//验证是否为邮箱
function isEmail(email) {
	var reg = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
	return reg.test(email);
}

//动态加入样式
function appendStyle(cssText) {
	var style = document.createElement("style");
	cssText = "*{margin:0;padding:0;}";
	style.type = "text/css";
	style.textContent = cssText;
	document.getElementsByTagName("head").item(0).appendChild(style);
}

//输出所有组合
function combination(arr, n) {
	var len = arr.length;
	if(n == len - 1) {
		console.log(arr.join());
	} else {
		var index = n;
		for(var i = n; i < len; i++) {
			var temp = arr[index];
			arr[index] = arr[i];
			arr[i] = temp;
			combination(arr, n + 1);
		}
	}
}

//获取链接参数
function getQueryString(name) {
	var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
	var r = window.location.search.substr(1).match(reg);
	if(r != null) return unescape(r[2]);
	return null;
}

//模板引擎
function TempleteEngine(tpl, opts) {
	let reg = /&lt;%(.*?)%&gt;/g
	let cursor = 0 //用一个游标来标示js代码位置
	let code = 'var arr =[];'
	//方便console.log查看所以添加了一个换行符，最后的实现完你最好删除它。
	function add(str, isJs = false) {
		//增加了一个判断，判断是js的话不以字符串的形式放进数组。
		if(isJs) {
			code += 'arr.push(' + str + ');';
		} else {
			code += 'arr.push("' + str.replace(/"/g, '\"') + '");'
		}
		//字符串的替换操作是为了转义，js不允许在双冒号内再添加双冒号
	}
	while(match = reg.exec(tpl)) {
		//字符串截取，index显示的是匹配位置
		add(tpl.slice(cursor, match.index))
		add(match[1], true) //js代码需要特殊处理
		cursor = match.index + match[0].length
	}
	add(tpl.slice(cursor, tpl.length))
	code += 'return arr.join("")';
	return new Function(code.replace(/\t\n\r/g, '')).call(opts)
}
/**
 * 绑定按钮事件
 * @param {Object} win
 * @param {Object} doc
 */
(function(win, doc) {
	var keyGroups = {};
	keyGroups.init = function(opts) {
		if(opts) {
			doc.body.addEventListener('keydown', function(ev) {
				//按住ctrl键，点击其他键触发
				if(ev.ctrlKey) {
					var c = String.fromCharCode(ev.keyCode);
					var k1 = c.toLocaleLowerCase();
					var k2 = c.toLocaleUpperCase();
					opts[k1] && opts[k1]();
					opts[k2] && opts[k2]();
				}
			});
		}
	};
	win.keyGroups = keyGroups;
})(window, document);

function copyVal(val) {
	var el = document.createElement("input");
	el.value = val;
	document.body.appendChild(el);
	el.select();
	document.execCommand('copy');
	document.body.removeChild(el);
}
/**
 * 复制内容至剪切板，兼容ios手机
 * @param {Object} val
 */
function copyVal(val) {
	var el = document.getElementById("myInput");
	if(el) {
		el.value = val;
	} else {
		el = document.createElement("input");
		el.id = 'myInput';
		el.value = val;
		el.readOnly = 'readonly';
		el.style.cssText = "position: absolute;top: -1000px;"
		document.body.appendChild(el);
	}
	el.select();
	el.setSelectionRange(0, val.length);
	document.execCommand('copy');
}
/**
 * 数字相乘
 * @param {Object} a
 * @param {Object} b
 */
function multiply(a, b) {
	var astr = a.toString(),
		bstr = b.toString(),
		anum = 0,
		bnum = 0;
	if(astr.indexOf('.') > -1) {
		anum = astr.length - astr.indexOf('.') - 1;
		a = parseInt(astr.replace('.', ''), 10);
	}
	if(bstr.indexOf('.') > -1) {
		bnum = bstr.length - bstr.indexOf('.') - 1;
		b = parseInt(bstr.replace('.', ''), 10);
	}
	return a * b / Math.pow(10, anum + bnum);
}


/**
 * 弹出提示
 */

function toast() {
	function getCssStr(obj) {
		var str = "";
		for(var key in obj) {
			str += key + ':' + obj[key] + ';'
		}
		return str;
	}

	var arr = arguments;
	var str = "";
	var time = 3000;
	var width = 400;

	if(typeof arguments[0] === 'string') {
		str = arguments[0];
	} else if(typeof arguments[0] === 'object') {

		var obj = arguments[0];

		obj.str && (str = obj.str);
		obj.time && (time = obj.time);
		obj.width && (width = obj.width);

	}

	var id = 'common_toast';
	var el = document.getElementById(id);

	if(el && el.isShowing) {
		return;
	}

	if(!el) {

		el = document.createElement("div");
		el.id = id;

		el.style.cssText = getCssStr({
			"position": "fixed",
			"top": "50%",
			"left": "50%",
			"background": "#000000",
			"opacity": 0,
			"color": "#FFFFFF",
			"width": width + "px",
			"padding": "20px",
			"transform": "translate(-50%,-50%)",
			"-webkit-transform": "translate(-50%,-50%)",
			"text-align": "center",
			"border-radius": "5px",
			"transition": "opacity 1s ease-in-out",
			"-webkit-transition": "opacity 1s ease-in-out"
		});
		document.body.appendChild(el);
	}
	el.innerHTML = str;
	el.style.opacity = '0.9';
	el.isShowing = true;

	setTimeout(function() {
		el.style.opacity = '0';
		el.isShowing = false;
	}, time);

}
/**
 * 随机红包
 * @param {Object} money 红包金额
 * @param {Object} num 红包数
 */
function getRedPacket(money, num) {
	var result = [];
	while(1) {
		if(num == 1) {
			result.push((Math.round(money * 100) / 100).toFixed(2) * 1);
			break;
		}
		var max = money / num * 2;
		var min = 0.01;
		var ranMoney = (max * Math.random()).toFixed(2);
		ranMoney = ranMoney < min ? min : ranMoney;
		result.push(ranMoney * 1);
		money = money - ranMoney;
		num--;
	}
	return result;
}

```
