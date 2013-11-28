<h1>Validator</h1>
=========

<p>Form Validator</p>

<h2>Init :</h2>

<pre>
var F = Validator('form表单name值',{
		together : false, //默认遇错误打断，显示单条错误信息,默认为false（只显示一条）
		errShow : 'alert', //错误提示，默认为alert，支持字符串(alert,single,multiple),自定义function(string || array())
		errBox : 'error_strings', //错误消息class，默认为form表单中的 .error_strings
        errPar : 'li', //单个表单元素的父级元素，用于定位错误的位置 li > (span > input ) ~ span.error_strings
		timely : false //实时判断，是否失去焦点以及change判断
});

//添加验证规则，传参为（二维数组或者一维数组，单条验证规则必须符合传参规则），
即：[字段名,判断条件,报错提示或者函数]


二维数组形式

F.addRule([
    ["username","required",'姓名不能为空'],
    ["username","regex=/^[A-Za-z]+$/",'只能是a-z'],
    ["username","minlength=3",'姓名必须大于3个字符'],
    ["username","maxlength=10",'姓名必须小于10个字符'],
    ["email","required",'邮箱必填'],
    ["email","email",'邮箱格式']
]);

一维数组形式（只能定义一条规则）

F.addRule(["username","required",'姓名不能为空']);



其中第三个参数可以是function,比如

F.addRule([
    ["username","required",function(){ alert('姓名不能为空') }],
	.....
]);

addRule可以动态添加，并通过removeRule来移除（removeRule同样可以采用二维数组和一维数组），比如

F.removeRule([
	['username','required'],
	['username','regex']
	......
])
规则：[字段名,验证规则] ， 如果不添加验证规则，默认会移除该字段的所有验证，如
F.removeRule(['username']) 会移除所有username字段的所有验证规则



全局的报错也支持function(接收参数为错误的数组),

errShow : function(data){
    var wrongList = document.getElementById('wrongList'), html = [];
    wrongList.innerHTML = '';
    for(var i =0; i < data.length ; i += 1){
        html.push('<li>'+data[i].msg+'</li>');
    }
    wrongList.innerHTML = html.join('');
}


</pre>

<h2>API :</h2>


regex : 正则， （regex=/^[A-Za-z]+$/ ） <br/>
required : 必填内容，针对input，textarea <br/>
minlength : 最小字符长度 （minlength=3）<br/>
maxlength : 最大字符长度 （maxlength=10）<br/>
number : 数字<br/>
alpha : 字母（大小写都可）<br/>
string : 字母，数字，下划线<br/>
email : 邮件格式<br/>
telephone : 电话<br/>
mobile : 手机<br/>
greaterthan : 大于某个值，或者某个input中的值 （greaterthan=5 或者 greaterthan=字段名 ）<br/>
lessthan : 小于某个值，用法同上<br/>
equal : 等于某个值，或者数组(数组以|分隔)中的某个值，或者字段  （equal=66 或者 equal=字段名 或者  equal='aa|bb|cc' ）<br/>
unequal ： 不等于某个值，用法同上<br/>

notselect ：不能选择的值，指的是select中option的value,或者 radio/checkbox数组中某一个的value （notselect=字符串或者数字或者数组）<br/>
shouldselect : 必须选中，用法同上<br/>

minselect : 最少选中几项<br/>
maxselect : 最多选择几项<br/>
