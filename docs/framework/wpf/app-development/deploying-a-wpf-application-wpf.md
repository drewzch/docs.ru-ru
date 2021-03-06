---
title: Развертывание приложений WPF
ms.date: 03/30/2017
helpviewer_keywords:
- WPF applications [WPF], deployment
- deployment [WPF], applications
ms.assetid: 12cadca0-b32c-4064-9a56-e6a306dcc76d
ms.openlocfilehash: d67919ba38c2e306672966ddc2f62140ef92b638
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636306"
---
# <a name="deploying-a-wpf-application-wpf"></a>Развертывание приложений WPF
После сборки приложений Windows Presentation Foundation (WPF) их необходимо развернуть. Windows и .NET Framework включают несколько технологий развертывания. Технология развертывания, используемая для развертывания приложения WPF, зависит от типа приложения. В этом разделе представлен краткий обзор каждой технологии развертывания и их использование в сочетании с требованиями к развертыванию каждого типа приложения WPF.  

<a name="Deployment_Technologies"></a>   
## <a name="deployment-technologies"></a>Технологии развертывания  
 Windows и .NET Framework включают несколько технологий развертывания, в том числе:  
  
- развертывание с помощью XCopy;  
  
- Развертывание установщик Windows.  
  
- Развертывание ClickOnce.  
  
<a name="XCopy_Deployment"></a>   
### <a name="xcopy-deployment"></a>Развертывание с помощью XCopy  
 Развертывание с помощью XCopy означает использование программы командной строки XCopy для копирования файлов из одного расположения в другое. Развертывание с помощью XСopy подходит для указанных ниже случаев.  
  
- Приложение является самодостаточным. Для его запуска не требуется обновлять клиент.  
  
- Файлы приложения должны быть перемещены из одного расположения в другое, например из расположения сборки (локальный диск, файловый ресурс UNC и т. д.) в расположение публикации (веб-сайт, файловый ресурс UNC и т. д.).  
  
- Для приложения не требуется интеграция в оболочку (добавление значка в меню "Пуск", на рабочий стол и т. д.).  
  
 Хотя технология XCopy подходит для простых сценариев развертывания, ее недостаточно, когда требуется выполнить более сложное развертывание. В частности, при использовании XCopy могут возникать дополнительные затраты на создание, выполнение и поддержку скриптов для надежного управления развертыванием. Кроме того, XCopy не поддерживает управление версиями, удаление или откат.  
  
<a name="Windows_Installer"></a>   
### <a name="windows-installer"></a>установщик Windows  
 Установщик Windows позволяет упаковывать приложения как автономные исполняемые файлы, которые можно легко распространить на клиенты и запустить. Кроме того, установщик Windows устанавливается вместе с Windows и обеспечивает интеграцию с рабочим столом, меню «Пуск» и панелью управления «программы».  
  
 Установщик Windows упрощает установку и удаление приложений, но не предоставляет средства для обеспечения актуальности установленных приложений с точки зрения управления версиями.  
  
 Дополнительные сведения о установщик Windows см. в разделе [установщик Windows развертывание](/visualstudio/deployment/deploying-applications-services-and-components#create-an-installer-package-windows-desktop).
  
<a name="ClickOnce_Deployment"></a>   
### <a name="clickonce-deployment"></a>развертывание ClickOnce  
 Технология ClickOnce позволяет развертывать веб-приложения для приложений, не являющихся веб-приложениями. Приложения публикуются на веб-серверах или файловых серверах и развертываются с них. Несмотря на то, что технология ClickOnce не поддерживает полный спектр клиентских функций, выполняемых установщик Windows приложениями, она поддерживает подмножество, включающее в себя следующее:  
  
- интеграция в меню "Пуск" и элемент панели управления "Программы";  
  
- управление версиями, откат и удаление;  
  
- режим интернет-установки, в котором приложение всегда запускается из места развертывания;  
  
- автоматическое обновление при выходе новых версий;  
  
- регистрация расширений файлов.  
  
 Дополнительные сведения о ClickOnce см. в статье [безопасность и развертывание ClickOnce](/visualstudio/deployment/clickonce-security-and-deployment).  
  
<a name="Deploying_WPF_Applications"></a>   
## <a name="deploying-wpf-applications"></a>Развертывание приложений WPF  
 Параметры развертывания для приложения WPF зависят от типа приложения. С точки зрения развертывания WPF имеет три основных типа приложений:  
  
- автономные приложения;  
  
- приложения, полностью состоящие из кода [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] разметки;  
  
- Приложения браузера XAML (XBAP).  
  
<a name="Deploying_Standalone_Applications"></a>   
### <a name="deploying-standalone-applications"></a>Развертывание автономных приложений  
 Автономные приложения развертываются с помощью ClickOnce или установщик Windows. В любом случае для запуска автономных приложений требуется полное доверие. Полное доверие автоматически предоставляется автономным приложениям, которые развертываются с помощью установщик Windows. Автономные приложения, развернутые с помощью ClickOnce, не получают полного доверия автоматически. Вместо этого в ClickOnce отображается диалоговое окно с предупреждением системы безопасности, которое пользователи должны принять перед установкой автономного приложения. Если предупреждение принято, автономное приложение устанавливается и ему предоставляется полное доверие. В противном случае автономное приложение не устанавливается.  
  
<a name="Deploying_Markup_Only_XAML_Applications"></a>   
### <a name="deploying-markup-only-xaml-applications"></a>Развертывание приложений XAML, содержащих только разметку  
 Страницы, [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] только разметки, обычно публикуются на веб-серверах, например HTML-страницах, и их можно просматривать с помощью Internet Explorer. Страницы [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], содержащие только разметку, запускаются в изолированной среде (в режиме безопасности с частичным доверием) с ограничениями, которые определяются набором разрешений зоны Интернета. Это обеспечивает эквивалентную изолированную среду безопасности для веб-приложений на основе HTML.  
  
 Дополнительные сведения о безопасности приложений WPF см. в разделе [Security](../security-wpf.md).  
  
 Страницы [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] только разметки могут быть установлены в локальную файловую систему с помощью XCopy или установщик Windows. Эти страницы можно просмотреть с помощью Internet Explorer или проводника Windows.  
  
 Дополнительные сведения о XAML см. в разделе [Общие сведения о языке XAML](../../../desktop-wpf/fundamentals/xaml.md).  
  
<a name="Deploying_XAML_Browser_Applications"></a>   
### <a name="deploying-xaml-browser-applications"></a>Развертывание приложений браузера XAML  
 XBAP — это скомпилированные приложения, для которых требуется развернуть следующие три файла:  
  
- *имя_приложения*.exe — исполняемый файл приложения сборки;  
  
- *имя_приложения*.xbap — манифест развертывания;  
  
- *имя_приложения*.exe.manifest — манифест приложения.  
  
> [!NOTE]
> Дополнительные сведения о манифестах развертывания и приложений см. в разделе [Построение приложения WPF](building-a-wpf-application-wpf.md).  
  
 Эти файлы создаются при построении XBAP. Дополнительные сведения см. в разделе [Практическое руководство. Создание нового проекта приложения обозревателя WPF](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/bb628663(v=vs.100)). Как и страницы [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] только разметки, XBAP обычно публикуются на веб-сервере и просматриваются с помощью Internet Explorer.  
  
 XBAP можно развертывать на клиентах с помощью любых методов развертывания. Однако рекомендуется использовать технологию ClickOnce, так как она предоставляет следующие возможности:  
  
1. автоматическое обновление при публикации новой версии;  
  
2. Права на повышение прав для XBAP, выполняющегося с полным доверием.  
  
 По умолчанию средство ClickOnce публикует файлы приложений с расширением DEPLOY. Это поведение может привести к затруднениям, но его можно отключить. Дополнительные сведения см. в разделе [Вопросы настройки сервера и клиента в развертываниях ClickOnce](/visualstudio/deployment/server-and-client-configuration-issues-in-clickonce-deployments).  
  
 Дополнительные сведения о развертывании приложений браузера XAML (XBAP) см. в разделе [Общие сведения о приложениях браузера WPF XAML](wpf-xaml-browser-applications-overview.md).  
  
<a name="Installing__NET_Framework_3_0"></a>   
## <a name="installing-the-net-framework"></a>Установка платформы .NET Framework  
 Для запуска приложения WPF на клиенте должна быть установлена платформа Microsoft .NET Framework. Internet Explorer автоматически определяет, устанавливаются ли клиенты с .NET Framework при просмотре приложений, размещаемых в браузере WPF. Если .NET Framework не установлен, Internet Explorer предлагает пользователям установить его.  
  
 Чтобы определить, установлена ли .NET Framework, Internet Explorer включает приложение загрузчика, зарегистрированное в качестве обработчика MIME для файлов содержимого со следующими расширениями: XAML, XPS, XBAP. и. Application. Если вы перейдете к этим типам файлов и на клиенте не установлен .NET Framework, приложение начального загрузчика запрашивает разрешение на его установку. Если разрешение не предоставлено, ни .NET Framework, ни приложение не установлено.  
  
 Если предоставлено разрешение, Internet Explorer скачивает и устанавливает .NET Framework с помощью Microsoft фоновая интеллектуальная служба передачи (BITS). После успешной установки .NET Framework изначально запрошенный файл открывается в новом окне браузера.  
  
 Дополнительные сведения см. в разделе [Развертывание .NET Framework и приложений](../../deployment/index.md).  
  
## <a name="see-also"></a>См. также:

- [Построение приложения WPF](building-a-wpf-application-wpf.md)
- [Security](../security-wpf.md)
