# 阿里云短信SDK

项目源自于：
github.com/sanxia/gsms

由于阿里云最近的更新不少参数发生变化，所以在源项目基础上进行了修正。现在的适用于阿里云2017-05-25的版本。

# 使用

```
package main

import (
	"aliyunsms/gsms"
	"fmt"
	"math/rand"
	"time"
)

const (
	ACCESSID  = "LTAIh0Gm**********"               //更换成你自己的
	ACCESSKEY = "Fc0mPR42CNyS********************" //更换成你自己的

	REGIONID = "cn-hangzhou"
	SIGNNAME = "中天不动产"        //更换成你自己的
	SMS_CODE = "SMS_78430056" //更换成你自己的
)

var smsProvider gsms.SmsProvider

func main() {

	phone := 18621229155             //提交的电话号码
	code := int32(RangeRand(999999)) //获取随机验证码

	result, err := AliyunSmsSend(fmt.Sprintf("%d", phone), fmt.Sprintf("%d", code))
	if err != nil {
		fmt.Println("err: %s, %s", result.Code, err)
		fmt.Println("获取验证码失败，请重试。")

	} else {
		fmt.Println("se: %s,%s", result.Code, result.Message)
		fmt.Println("获取验证码成功。")

	}

}

// mobiles 接收短信的手机
// code 对应的模板替换内容
func AliyunSmsSend(mobiles string, code string) (*gsms.SmsResult, error) {

	smsProvider = gsms.NewAliyunSms(ACCESSID, ACCESSKEY, REGIONID, SIGNNAME)
	smsProvider.SetTemplateCode(SMS_CODE)
	smsProvider.SetTemplateParam(gsms.SmsTemplateParam{
		Code: code, //修改成你对应的模板码
	})
	return smsProvider.Send(mobiles)
}

//生成规定范围内的整数
//设置起始数字范围，0开始,n截止
func RangeRand(n int) int {

	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	return r.Intn(n)

}

```

