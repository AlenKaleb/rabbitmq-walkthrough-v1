# WebApplicationEntrypoint/WebApplicationEntrypoint.csproj

## Propósito
Define o projeto ASP.NET Core que expõe APIs e hospeda os workers.

## Código anotado

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>net9.0</TargetFramework>
        <CopyRefAssembliesToPublishDirectory>false</CopyRefAssembliesToPublishDirectory>
        <DockerDefaultTargetOS>Linux</DockerDefaultTargetOS>
        <DockerComposeProjectPath>..\docker-compose.dcproj</DockerComposeProjectPath>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="9.0.5" />
        <PackageReference Include="Microsoft.VisualStudio.Azure.Containers.Tools.Targets" Version="1.21.2" />
        <PackageReference Include="Polly" Version="8.5.2" />        
        <PackageReference Include="RabbitMQ.Client" Version="7.1.2" />        
        <PackageReference Include="RestSharp.Serializers.NewtonsoftJson" Version="112.1.0" />        
        <PackageReference Include="System.Data.SqlClient" Version="4.9.0" />
    </ItemGroup>

    <ItemGroup>
      <ProjectReference Include="..\RabbitMQWalkthrough.Core\RabbitMQWalkthrough.Core.csproj" />
    </ItemGroup>

</Project>
```

### Anotações técnicas
- **`Microsoft.NET.Sdk.Web`**: habilita pipeline web ASP.NET.
- **`DockerComposeProjectPath`**: integra com o projeto de compose para desenvolvimento local.
- **Referência ao Core**: compartilha infraestrutura de mensageria, métricas e dados.
- **RuntimeCompilation**: facilita ajustes em views durante desenvolvimento.
