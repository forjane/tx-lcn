### tx-lcn tc端

微服务之间通信时，通过在请求头中传递x-group-id(事务组ID)来做关联

发起方事务传播不能设置为propagation=DTXPropagation.SUPPORTS

### tx-lcn tm端

t_tx_exception 异常表

ex_state: 待处理0  已处理1

registrar: 未知错误-1   通知事务单元失败0   询问事务状态失败1   通知事务组失败2    TCC清理事务失败3  TXC 撤销日志失败4



###源码分析###
事务发起方通知TM管理器事务状态：TransactionControlTemplate.notifyGroup, TM接受代码SimpleTransactionManager.notifyTransaction

事务参与方执行完成后通知TM管理器：TransactionControlTemplate.joinGroup, TM接受代码JoinGroupExecuteService.execute

TM管理通知事务参与者提交或回滚：DefaultNotifiedUnitService.execute

事务切面代码：DTXServiceExecutor.transactionRunning

MessageConstants netty通讯指令
RpcConfig waitTime TM-TC调用超时时间配置


###配置###
tx-lcn.springcloud.loadbalance.enabled=true

集群环境下，发起方A多次调用参与方B，A每次都调用B的同一个服务节点，避免lcn模式资源占用死锁