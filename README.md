# azure-devops-pipeline-chaining-yaml

 Example of how to configure an Azure DevOps chaining Pipelines (Build & Release) with yml files.

## Code

Build pipeline code - builds are triggered by commiting code to the master branch

```yml
trigger:
    - master

pool:
    vmImage: 'ubuntu-latest'

steps:
    - script: echo Hello, world!
      displayName: 'Run a one-line script'
    - task: CopyFiles@2
      inputs:
        SourceFolder: 'src'
        Contents: 'index.html'
        TargetFolder: '$(Build.ArtifactStagingDirectory)/'
    - task: PublishBuildArtifacts@1
      inputs:
        PathtoPublish: '$(Build.ArtifactStagingDirectory)'
        ArtifactName: 'drop'
        publishLocation: 'Container'
```

Release pipeline code - releases are trigged by the execution of the build pipeline

```yml
pool:
    vmImage: 'ubuntu-latest'

resources:
  pipelines:
  - pipeline: 'chaining-yaml - build pipeline'
    source: 'chaining-yaml - build pipeline'
    trigger:
      enabled: true
steps:
    - download: 'chaining-yaml - build pipeline'
      artifact: 'drop'
```

## Demo

* [Build Pipeline](https://dev.azure.com/infladera/Public/_build?definitionId=5&_a=summary)

* [Release Pipeline](https://dev.azure.com/infladera/Public/_build?definitionId=7&_a=summary)

## License

MIT - See [LICENSE](LICENSE) for more information.
