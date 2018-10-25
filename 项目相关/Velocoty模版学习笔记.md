# Velocoty模版学习笔记

```velocity
## 变量声明
#set($变量="值")
#set($a="Velocity")

$name
为空时打印变量本身。
$!name
为空时打印空字符串（不打印任何内容）。
${name}
类似 $name，为空时原样打印。但可以将变量和连续的字符串分隔，例如：${name}space。
$!{name}
类似 $!name，为空时打印空字符串，但可以将变量和连续的字符串分隔。例如： $!{name}space。
$name$!name${name}$!{name}
为空时打印：	"$name"	""	"${name}"	""
带花括号的属性/方法调用方式，属性/方法需要在花括号之内：
${cookie.name}
${request.getCookies()}

## 注释
## 单行注释
#* 
这是多行注释
*#

## 属性 对象.属性
$customer.Address
$purchase.Total

## 方法 对象.方法
$customer.getAddress()
$purchase.getTotal()
$page.setTitle( "My Home Page" )
$person.setAttributes( ["Strange", "Weird", "Excited"] )

## 不会被解析
#[[
#foreach ($woogie in $boogie)
  nothing will happen to $woogie
#end
]]#

## 输出
#foreach ($woogie in $boogie)
  nothing will happen to $woogie
#end

##条件的使用
#if( $foo < 10 )
    **Go North**
#elseif( $foo == 10 )
    **Go East**
#elseif( $bar == 6 )
    **Go South**
#else
    **Go West**
#end

## 循环
<ul>
#foreach( $product in $allProducts )
    <li>$product</li>
#end
</ul>

## 可用于中断 #foreach() 循环
#break


## include（只引进不解析）
#include( "one.gif","two.txt","three.htm" )

## Parse （引进并解析）
#parse( "me.vm" )


## 范围(range)
#foreach($item in [10..20])
    $item
#end

##对象 & 访问
#set($obj = {"key":"value", "name":"space"})
$obj.get("key")

#foreach(#item in $obj)
    $item
#end

##macros()定义函数

#macro(macroName)
    #subMacro("name", "value")
#end

#macro(subMacro $param1 $param2)
    this is sub macro($param1, $param2).
#end

## debug
#stop
停止模板引擎，在 Debug 时比较有用。
```

