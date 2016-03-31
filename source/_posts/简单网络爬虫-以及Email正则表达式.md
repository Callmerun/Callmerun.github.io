---
title: 简单网络爬虫 以及Email正则表达式
date: 2016-03-15T10:52:35.000Z
categories: Java
tags:
  - Java
  - 正则表达式
---

**需求：一个很简单的网络爬虫，把指定页面上的邮箱地址爬下来,过滤后符合规则的邮箱地址保存到一个指定TXT文件中**

# Java源码

```Java
package IO;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Robot {

    /**
     * @param args
     *    简单的网络爬虫
     *
     *    抓取指定网页里面所有的邮箱地址
     */
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        //String url = "https://www.douban.com/group/topic/24022171/";
        String url = "http://blog.sina.com.cn/s/blog_515617e60101e151.html";
        String filename = "D://result.txt";
        method(url, filename);
    }

    public static void method(String url_str, String writetofile) {
        BufferedReader in = null;
        BufferedWriter out = null;

        try {
            URL url = new URL(url_str);
            URLConnection conn = url.openConnection();

            // 匹配email的正则

            // String regex = "\\w+@\\w+\\.\\w*";
            // String regex ="^\\w+([-+.]\\w+)*@\\w+([-.]\\w+)*\\.\\w+([-.]\\w+)*$";
            // String check ="^([a-z0-9A-Z]+[-|_|\\.]?)+[a-z0-9A-Z]@([a-z0-9A-Z]+(-[a-z0-9A-Z]+)?\\.)+[a-zA-Z]{2,}$";
            // String check ="^([a-z0-9A-Z]+[-|\\.]?)+[a-z0-9A-Z]@([a-z0-9A-Z]+(-[a-z0-9A-Z]+)?\\.)+[a-zA-Z]{2,}$";

            // String regex ="[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}";//与下面的一句同样的效果
            // String regex = "[a-zA-Z0-9_-]+(\\.\\w+)*@\\w+\\.[a-z]+(\\.[a-z]+)*"; //

            // 特别注意：
            // ^   行的开头
            // $   行的结尾
            // 所以要去掉这两个都要去掉
            // String regex =
            // "^[a-zA-Z0-9]([a-zA-Z0-9]|_+[a-zA-Z0-9]|[a-zA-Z0-9]*\\.[a-zA-Z0-9])+@\\w+\\.[a-zA-Z]{1,4}\\.{0,1}[a-zA-Z]{0,4}$";
            String regex =
            "[a-zA-Z0-9]([a-zA-Z0-9]|_+[a-zA-Z0-9]|[a-zA-Z0-9]*\\.[a-zA-Z0-9])+@\\w+\\.[a-zA-Z]{1,4}\\.{0,1}[a-zA-Z]{0,4}";
            //上面的正则表达式的来源请看[正则表达式匹配邮箱账号](http://blog.csdn.net/c860_zy/article/details/9257011)

            in = new BufferedReader(
                    new InputStreamReader(conn.getInputStream()));
            out = new BufferedWriter(new FileWriter(writetofile));

            String len;
            while ((len = in.readLine()) != null) {
                // 正则对象，过滤内容
                Pattern p = Pattern.compile(regex);

                //匹配器对象
                Matcher m = p.matcher(len);

                //这里不能用if语句，思考
                while (m.find()) {
                    out.write(m.group());
                    out.newLine();
                    out.flush();
                }
            }

        } catch (MalformedURLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } finally {
            try {
                if (in != null)
                    in.close();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }

            try {
                if (out != null)
                    out.close();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }

        }
    }

}
```

> 注意事件：上面的正则表达式的来源请看[正则表达式匹配邮箱账号](http://blog.csdn.net/c860_zy/article/details/9257011)

# 结果
结果中的邮箱地址有一些`zhangshna.Mr@163.com,abc_Wang.dd@sian.com,abc_Wang.dd.cc@sian.com`这种类似的形式

```
530180782@qq.com
243678025@qq.com
......省略一部分结果
xingfa@xingfayeya.com
cjwlj@21cn.com
toocle01@netsun.com
chuangling@chuangling.net
zjykrc@163.com
hahdjx@163.com
5663@sohu.com
chenql_008@163.com
tsmoql@alibaba.com.cn
wxweijie@163.com
jinganmotor@jinganmotor.cn
cnjiangtai@126.com
ankson@126.com
newsinda@yahoo.com
jwf110@hotmail.com
jsob@jsob.cn
shfmsr@hotmail.com
LYC@hzlasiji.com
yawei@xinyajx.com
......省略一部分结果
sanjiangjx@alibaba.com.cn
mr.zhang.yao@163.com  特殊格式
xxtljx@126.com
mxjm@mxjm.com
bbxindas@sohu.com.cc
lvjuntx@qszt.net
wcdb@wcdb.com.cn
......省略一部分结果
tanhua977@sohu.com
nianfashaiwang@163.com
tz_cxh@sohu.com  特殊格式
2008103102@123.com
whcx258@sohu.com
wzlmv67997728@126.com
xin.fa168@163.com  特殊格式
baoxiuyu@yahoo.cn
wcf@xinda.js.cn
hongfu@wjhongfu.cn
xiejinhua29@yahoo.com.cn
hzsongchengmachine@126.com
zhangqiaoqiao0731@gmail.com
cuioyou@163.com
hbt@hbt.cn
kaixuankite@sohu.com
beigongpump@qq.com
juwp@wpdj.com
lcshxxgzzyxgs@3158.com
bt_longhua@sina.com
lianyou@13566170918.com
qxb555@vip.sohu.com
root@xsp2.com
cileeautoparts@163.com
108@china114bst.net
fjsly@publ.qz.fj
deying@deyingmotor.com
ko5186@126.com
jinniutx@njjn.com
lirenfushi@126.com
NO@NO.com
huada88@sohu.com
yaxinus@hotmail.com
admin@jsgdjc.com
zcjd@163.com
sales@jshaifeng.com
waitting16@163.com
info@haixia.cc
jetski777@hotmail.com
bode@tsxidi.com
lilujia@vip.sina.com  特殊格式
trade@yongshengwiremesh.com
qianzhen0523@sina.com
talent@kailirazor.com
management@3dmould.com
baoli@baoli.cn
czslyxgs@163.com
......省略一部分结果
```

结果中有一部分被省略，需要的自己运行一遍.....
