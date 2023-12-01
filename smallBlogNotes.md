# WalkThrough

## Docker

### Volume
* Путь до "монтированных" дисков - \\wsl$\docker-desktop-data\data\docker\volumes\

## Visual Studio Solution

### Dependency
Для обнаружения зависимостей Nuget пакетов необходимо просмотреть файл csproj раздел PackageReference. 
Это даст первый уровень включения в проект пакета. Пример:

```
<ItemGroup>
    <PackageReference Include="AutoMapper" Version="12.0.1" />
</ItemGroup>
```
В проекте есть файл `\obj\<имя проекта>.csproj.nuget.g.props`. В этом файл содержится вся информация относительно используемого nuget. В том числе, пути до папок, где лежат файлы пакетов. Обычно пути такие:\
`"C:\Users\<имя пользователя>\.nuget\packages\"` или `"$(UserProfile)\.nuget\packages\"`\
`"C:\Program Files (x86)\Microsoft Visual Studio\Shared\NuGetPackages\"`\
Для установления внутренних зависимостей каждого такого пакета, можно найти соответствующую папку по идеологии:\
`<путь до папки пакетов>\<имя пакета>\<версия>\`. \
В этой папке находятся все файлы пакета, в том числе и библиотеки. Нас интересует файл:\
`<имя пакета>.nuspec`
В этом файле (формата xml) в разделе **dependencies**  хранится информация о внутренних зависимостях. Она сгруппирована по целевым платформам CLR. Пример:\

```
<dependencies>
    <group targetFramework=".NETFramework4.6.1">
        <dependency id="Avalonia.BuildServices" version="0.0.16" exclude="Build,Analyzers" />
    </group>
    <group targetFramework=".NETCoreApp2.0">
        <dependency id="Avalonia.BuildServices" version="0.0.16" exclude="Build,Analyzers" />
    </group>
</dependencies>
```
Собственно, из этого файла можно получить зависимости пакета от других пакетов и библиотек.
Сами файлы библиотек тоже лежат в папке пакета, но только название папки может отличаться. \
Примеры:\
`lib\net461\<файлы>`\
`ref\net471\<файлы>`\

### SSL Certificate
Для разработки нужен подписать сертификат. Для этого используется `dotnet dev-certs`.  
Материал:  
* [Enforce HTTPS in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/security/enforcing-ssl?view=aspnetcore-8.0&tabs=visual-studio%2Clinux-ubuntu)
* [ASP.NET Core security topics](https://learn.microsoft.com/en-us/aspnet/core/security/?view=aspnetcore-6.0)
* [How to setup the dev certificate when using Docker in development](https://github.com/dotnet/AspNetCore.Docs/issues/6199)
#### IIE Express
Если броузер не способен подключиться к приложению, можно попробовать несколько вещей.  
```
cd 'C:\Program Files (x86)\IIS Express'
IisExpressAdminCmd.exe setupsslUrl -url:https://localhost:44387/ -UseSelfSigned
```
Полезные ссылки:  
* [How do I restore a missing IIS Express SSL Certificate?](https://stackoverflow.com/questions/20036984/how-do-i-restore-a-missing-iis-express-ssl-certificate/20048613#20048613)

### Containers  
Материал:  
* [Customize Docker containers in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/containers/container-build?view=vs-2022&ref=goatreview.com)

### Debug
Материал:  
* [Attaching to remote processes](https://github.com/dotnet/vscode-csharp/wiki/Attaching-to-remote-processes)


## PowerShell

### Запуск 
Для запуска из командной строки необходимо добавить аттрибуты при вызове powershell.exe. Вот так:
``` 
> powershell.exe -NonInteractive -NoProfile -ExecutionPolicy RemoteSigned -File "C:\...\<имя>.ps1" 
```

## Docker
