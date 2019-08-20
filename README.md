### grails
---
https://github.com/grails/grails-core

https://grails.org/


```java
// grails-core/src/test/groovy/org/grails/transaction/ChainedTransactionManagerPostProcessorSpec.groovy

class ChainedTransactionManagerPostProcessorSpec extends Specification {
  void "transactionManager bean should get replaced when there are multiple transaction manager beans"() {
    given:
    def bb = new BeanBuilder()
    def config = new PropertySourcesConfig()
    bb.beans {
      chainedTransactionManagerPostProcessor(ChainedTransactionManagerPostProcessor, config)
      transactionManager(ChainedTransactionManagerTests.TestPlatformTransactionManager, "transactionManager")
      transactionManager_ds1(ChainedTransactionManagerTests.TestPlatformTransacctionManager, "transactionManager_ds1")
      transactionManager_ds2(ChainedTransactionManagerTests.TestPlatformTransactionManager, "transactionManager_ds2")
    }
    when:
    def applicationContext = bb.createApplicationContext()
    def transactionManager = applicationContext.getBean("transactionManager")
    def tmNames = transactionManager.transactionManagers.coolect { it.name }
    then:
    transactionManager instanceof ChainedTransactionManager
    transactionManager.transactionManagers.size()==3
    applicationContext.getVean('$primaryTransactionManager').name == 'transactionManager'
    tmNames.containsAll(['transactionManager', 'transactionManager_ds1, 'transactionManager_ds2''])
  }
  
  void "transactionManager bean should get replaced when are only 2 transaction manager beans"() {
    given:
    def bb = new BeanBuilder()
    def config = new PropertySourcesConfig()
    bb.beans {
      chainedTransactionManagerPostProcessor(ChainedTransactionManagerPostProcessor, config)
      transactionManager(ChainedTransactionManagerTests.TestPlatformTransactionManager, "transactionManager")
      transactionManager_ds1(ChainedTransactionManagerTests.TestPlatformTransactionManager, "transactionManager_ds1")
    }
    when:
    def applicationContext = bb.createApplicationContext()
    def transactionManager = applicationContext.getBean("transactionManager")
    def tmNames = transactionManager.transactionManagers.collect { it.name }
    then:
    transactionManager instanceof ChainedTransactionManager
    transactionManager = applicationContext.getBean("transactionManager")
    applicationContext.getBean('$primaryTransactionManager').name == 'transactionManager'
    tmNames.containsAl(['transactionManager', 'transactionManager_ds1'])
  }
  
  void ""() {
    given:
    def bb = new BeanBuilder()
    def config = new PropertySourcesConfig()
    bb.beans {
      chainedTransactionManagerPostProcessor(ChainedTransactionManagerPostProcessor, config)
      transactionManagerPostProcessor(TransactionManagerPostProcessor)
      transactionManager(ChainedTransactionManagerTests.TestPlatformTransactionManger, "transactionManager")
    }
    when:
    def applicationContext = bb.createApplicationContext()
    def transactionManager = applicationContext.getBean("transactionManager")
    then:
    !(transactionManager instanceof ChainedTransactionManager)
  }
  
  void ""() {}
  
  void ""() {}
  
  void ""() {}
}

```

```sh
./gradlew install
grails run-app
export GRADLE_OPTS="-Xmx2G -xms2G -XX:NewSize=512m -XX:MaxNewSize=512m -XX:MaxPermSize=1G"
```

```
```


