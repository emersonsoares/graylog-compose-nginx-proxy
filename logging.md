# LTM Logging

### Configuraçao básica

#### Instale o pacote `LTM.Logging`

```powershell
Install-Package LTM.Logging
```

#### Configure o `log4net` no `[App/Web].config`

Primeiro, registre a secao de configuracao do `log4net` no `Web.config` em `/configuration/configSections`:

```xml
<configSections>
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler, log4net" />
</configSections>
```

Entao adicione a configuracao do `log4net` registrando o `Appender` do pacote `LTM.Logging`:

```xml
<log4net>
    <appender name="GraylogHttpAppender" type="LTM.Logging.GraylogHttpAppender, LTM.Logging">
        <url value="http://<graylog-server>/gelf" />
        <user value="<user>" />
        <password value="<password>" />
        <application value="<Application Name>" />
    </appender>
    <root>
        <level value="ALL" />
        <appender-ref ref="GraylogHttpAppender" />
    </root>
</log4net>
```

Utilize o `log4net` normalmente:

```c#
var myLogger = LogManager.GetLogger("MyLogger");

myLogger.Error("Some error", new Exception("Some exception"))
myLogger.Info("Informational log");
myLogger.Warning("Warning log");
myLogger.Debug("Some debug level log");
```

#### Logs Correlacionados

Para correlacionar logs utilize a classe `LTM.Logging.CorrelatedLog`:

```c#
var myLogger = LogManager.GetLogger("MyLogger");

var myCorrelationId = "XPTO159";

myLogger.Error(new CorrelatedLog(myCorrelationId, "Some error"), new Exception("Some exception"));
myLogger.Info(new CorrelatedLog(myCorrelationId, "Some informational log"));
myLogger.Warning(new CorrelatedLog(myCorrelationId, "Warning log"));
myLogger.Debug(new CorrelatedLog(myCorrelationId, "Some Debug log"));
```  
A classe `LTM.Logging.CorrelatedLog` e um `Dictionary<string, object>` entao voce pode adicionar quantas novas propriedades forem necessarias no objeto de log:

```c#
var myCorrelationId = "XPTO159";

var myBigLogEntry = new CorrelatedLog(myCorrelationId, "Starting transaction");

myBigLogEntry.Add(nameof(request.Parameters), request.Parameters);
```

