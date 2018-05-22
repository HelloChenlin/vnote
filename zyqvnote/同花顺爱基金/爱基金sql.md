# 爱基金sql  
#### 收益
获取当日收益  
`SELECT * FROM t_share_daylist_p where  vc_transactionaccountid='600104180339'`  

获取份额收益表  
`SELECT * FROM t_share_income where  vc_transactionaccountid='600104180339'`  
#### 交易
查询交易请求表 （交易对应的支付渠道）  
`SELECT * from t_trade_tradereq  where  vc_custid='100102347478'and vc_transactiondate='20180316'`  
`SELECT * from t_trade_tradereq  where  vc_appsheetserialno='00000000000167472896'`

查询用户开户渠道表(用户对应的默认渠道和渠道设置是否异常以及开户失败的次数)     
**（全渠道开户的表）**  
`SELECT * from t_acco_opencapitalmethod where vc_custid='100102347478'`  

查询交易账户表（主要是银行的信息和个人开户的信息）（切换渠道的表）  
`SELECT *from t_acco_tradeacco where vc_custid='100102347478'`  

查询快速赎回表数据(普通T+0)  
`select * from t_trade_intimereq where vc_applyserial ='00000000000169727806'`    
`select * from t_trade_intimereq where vc_bankacconame='章御强'`  
```
"c_capitaloutstate" IS '划款状态2-未划款，3-已发起划款，4-划款成功，5-划款失败6-作废';
"c_confirmflag" IS '3-实时确认成功，0-确认失败，1-确认成功';
```
查询钱包请求数据  
`select * from t_realtime_tradereq where vc_custid='100103890803'`
```
c_sendflag   '发送标志.0-需要发送1-不需要发送.';
c_checkflag  申请校验.0-通过检查；1-不需要发送.';
c_confirmflag 确认处理标志 .0 未确认1 已撤单2 未完全确认3 确认成功4 确认失败5 认购交易确认6 作废.';
```


#### 日志
查询日志表  
`select requestparams,seq_id,requestxml,fundcode,responsexml,logtime,responsemsg from t_logbean_201803 where custid='100102347478'`      
`select * from t_logbean_201804 where custid='100103610045'`
#### 信息
查询客户信息表（客户的基本信息，字段较多）  
`select  ni_passworderrorcount,vc_lockdate from t_acco_custacco as t where vc_custid='100100409245'`  
帐户申请表（绑定银行卡成功后，写入开卡申请信息;存储开户信息，包括TA账户）  
`SELECT * FROM t_trade_accoreq WHERE vc_custid='100100006777'`

#### 银行和渠道
查询客户的银行账户信息 (银行账号，交易账号，是否销户，银行开户证件号码，销户日期)    
`SELECT * FROM  t_acco_bankacco where  vc_custid='100103318255'`  
查询银行对应的支付渠道  
`select * from t_parm_trustsignpay_support where vc_bankcode='002'`  
查询同花顺中银行代码 和银行联行号前几位（bankcode,branchcode）  
`select * from t_parm_bankinfo_support where vc_bankcode='002'`  
查询连接  
`select * from t_parm_bankinfo_support  t1 inner JOIN t_parm_trustsignpay_support  t2
on t1.vc_bankcode=t2.vc_bankcode
where t1.vc_bankcode='002'`

查询银行信息（branchbank 联行号）  
`select * from t_parm_bankinfo where vc_bankname like '%华夏%' `  
查询同花顺银行代码 在支付渠道对应的银行代码  
`select vc_bankcode,vc_bankcoderel from  v_parm_banksupport where c_capitalmethod='1'`

查询支付渠道支持的银行的列表  
`select * from t_parm_trustsignpay_support t1  inner JOIN t_parm_bankinfo_support t2  
on t1.vc_bankcode=t2.vc_bankcode 
where c_capitalmethod='1' `
