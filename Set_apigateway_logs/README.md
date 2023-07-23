# Set Up Logs for APIGATEWAY when we call to lambda function

### Folow below steps.

#### IAm User

![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/IAM%20role/1log.png)

![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/IAM%20role/2log.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/IAM%20role/3log.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/IAM%20role/4log.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/IAM%20role/5log%20created.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/IAM%20role/6%20log%20API-log%20search.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/IAM%20role/7%20log%20copy%20arn%20of%20API-log%20role.png)

#### APIGATEWAY Settings

![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/Apigateway/1%20api%20gateway%20setting.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/Apigateway/2%20apigateway%20%20paste%20arn.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/Apigateway/3%20apigateway%20got%20to%20stage%20and%20logs-tracing.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/Apigateway/4%20apigateway%20%20select%20log%20type%20.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/Apigateway/5%20apigateway%20redeploye%20.png)


#### CLOUDWATCH

![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/cloud%20watch/1%20cloud%20watch%20log%20group.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/cloud%20watch/2%20cloud%20watch%20search%20apigateway%20in%20log%20group.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/cloud%20watch/3%20cloud%20watch%20group%20open%20.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/cloud%20watch/4%20cloud%20watch%20invoke%20apigateway%20%20.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/cloud%20watch/5%20cloud%20watch%20check%20the%20log%20is%20generated.png)
![](https://github.com/faridelya/Connect_AWS_EFS_with_Lambda/blob/main/Set_apigateway_logs/images/cloud%20watch/6%20colud%20watch%20log.png)
