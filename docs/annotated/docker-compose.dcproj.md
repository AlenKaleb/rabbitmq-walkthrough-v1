# docker-compose.dcproj

## Propósito
Arquivo de projeto do Visual Studio para orquestrar o Docker Compose.

## Código anotado

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="15.0" Sdk="Microsoft.Docker.Sdk">
  <PropertyGroup Label="Globals">
    <ProjectVersion>2.1</ProjectVersion>
    <DockerTargetOS>Linux</DockerTargetOS>
    <ProjectGuid>fd9669c0-be53-42c0-977e-94d6ac15549b</ProjectGuid>
    <DockerLaunchAction>LaunchBrowser</DockerLaunchAction>
    <DockerServiceUrl>{Scheme}://localhost:{ServicePort}</DockerServiceUrl>
    <DockerServiceName>webapplicationentrypoint</DockerServiceName>
  </PropertyGroup>
  <ItemGroup>
    <None Include="docker-compose.override.yml">
      <DependentUpon>docker-compose.yml</DependentUpon>
    </None>
    <None Include="docker-compose.yml" />
    <None Include=".dockerignore" />
  </ItemGroup>
</Project>
```

### Anotações técnicas
- **Integração com VS**: permite F5/Debug direto do IDE.
- **`DockerServiceName`**: define serviço principal para abrir no browser.
