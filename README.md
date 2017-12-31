# Wargame.kr-login-filtering

# Question
* I have accounts. but, it's blocked. can you login bypass filtering?

# Source
```javascript
<?php

if (isset($_GET['view-source'])) {
    show_source(__FILE__);
    exit();
}

/*
create table user(
 idx int auto_increment primary key,
 id char(32),
 ps char(32)
);
*/

 if(isset($_POST['id']) && isset($_POST['ps'])){
  include("../lib.php"); # include for auth_code function.

  mysql_connect("localhost","login_filtering","login_filtering_pz");
  mysql_select_db ("login_filtering");
  mysql_query("set names utf8");

  $key = auth_code("login filtering");

  $id = mysql_real_escape_string(trim($_POST['id']));
  $ps = mysql_real_escape_string(trim($_POST['ps']));

  $row=mysql_fetch_array(mysql_query("select * from user where id='$id' and ps=md5('$ps')"));

  if(isset($row['id'])){
   if($id=='guest' || $id=='blueh4g'){
    echo "your account is blocked";
   }else{
    echo "login ok"."<br />";
    echo "Password : ".$key;
   }
  }else{
   echo "wrong..";
  }
 }
?>
<!DOCTYPE html>
<style>
 * {margin:0; padding:0;}
 body {background-color:#ddd;}
 #mdiv {width:200px; text-align:center; margin:50px auto;}
 input[type=text],input[type=[password] {width:100px;}
 td {text-align:center;}
</style>
<body>
<form method="post" action="./">
<div id="mdiv">
<table>
<tr><td>ID</td><td><input type="text" name="id" /></td></tr>
<tr><td>PW</td><td><input type="password" name="ps" /></td></tr>
<tr><td colspan="2"><input type="submit" value="login" /></td></tr>
</table>
 <div><a href='?view-source'>get source</a></div>
</form>
</div>
</body>
<!--

you have blocked accounts.

guest / guest
blueh4g / blueh4g1234ps

-->
```
# Solution
1. 선뜻보면 어려운 문제같아 보이지만 사실은 매우 간단한 문제이다

2. 소스를 자세히보면 블럭된 아이디와 패스워드를 제공해준다

3. PHP에서 필터링 우회방법은 여러가지가잇는데 이문제에서는 대소문자를 이용해 공략한다

4. 소스를보면 대소문자 구별하는 부분을 찾을수 없다

5. ID: Guest PS: guest 혹은 ID:Blueh4g PS:blueh4g1234ps

6. Flag: 5b58de7c1719712522d22654c3b8f9be80705f01
