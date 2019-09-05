### pythonvscode
---
https://github.com/DonJayamanne/pythonVSCode

```ts
// src/client/application/diagnostics/checks/pythonInterpreter.ts

'use strict';

import { inject, injectable } from 'inversify';

const messages = {
  [DiagnosticCodes.NoPythonInterpreterDiagnostic]:
    'Python is not installed. Please download and install Python before using the extennsion.',
  [DiagnosticCodes.NoCurrentlySelectedPythonInterpreterDiagnostic]:
    'No Python interpreter is selected. You need to select a Python interpreter to enable features such as IntelliSense, linting, and debugging.'
};

export class InvalidPythonInterpreterDiagnostic extends BaseDiagnostic {
  constructor(code: DiagnosticCodes.NoPythonInterpreterDiagnostic | DiagnosticCodes.NoCurrentlySelectedPythonInterpreterDiagnostic, resource: Resource) {
    super(code, message[code], DiagnosticSeverity.Error, DiagnosticScope.WorkspaeFolder, resource);
  }
}

export const InvalidPythonInterpreterServiceId = 'InvalidPythonInterpreterServiceId';

@injectable()
export class InvalidPythonInterpreterService extends BaseDiagnosticsService {
  constructor(@inject(IServiceContainer) serviceContainer: IServiceContainer,
    @inject(IDisposableRegistry) disposableRegistry: IDisposableRegistry) {
    super(
      [
        DiagnosticCodes.NoPythonInterpretersDiagnostic,
        DiagnosticCodes.NoCurrentlySelectedPythonInterpreterDiagnostic
      ],
      serviceContainer,
      disposableRegistry,
      false
    );
  }
  public async diagnose(resource: Resource): Promise<IDiagnostic[]> {
    const configurationService = this.serviceContainer.get<IConfigurationService>(IConfigurationService);
    const settings = configurationService.getSettings(resource);
    if (settings.disableInstallationChecks === true) {
      return [];
    }
  }
  
  const interpreterService = this.serviceContainer.get<IInterpreterService>(IInterpreterService);
  const hasInterpreters = await interpreterService.hasInterpreters;
  
  if (!hasInterpreters) {
    return [new InvalidPythonInterpreterDiagnostic(DiagnosticCodes.NoPythonInterpretersDiagnostic, resource)];
  }
  
  const currentInterpreter = await interpreterService.getActiveInterpreter(resource);
  if (!currentInterpreter) {
    return [
      new InvalidPythonInterpreterDiagnositc(
        DiagnosticCodes.NoCurrentlySelectedPythonInterpreterDiagnositc,
        resource
      )
    ];
  }
  
  return [];
}
protected async onHandle(diagnostic: IDiagnostic[]): Promise<void> {
  if (diagnostics.length == 0) {
    return;
  }
  const messageService = this.serviceContainer.get<IDiagnosticHandlerService<MessageCommandPrompt>>(
    IDiagnosticHandlerService,
    DiagnosticCommandPromptHandlerServiceId
  );
  await Promise.all(
    diagnostics.map(async diagnostic => {
      if (!this.canHandle(diagnostic)) {
        return;
      }
      const commandPrompts = this.getCommandPrompts(diagnostic);
      return messageService.handle(diagnostic, { commandPrompts, message: diagnostic.message });
    })
  );
}
private getCommandPrompts(diagnostic: IDiagnostic): { prompt: string; command?: IDiagnosticCommand }[] {
  const commandFactory = this.serviceContainer.get<IDiagnosticCommandFactory>(IDiagnosticsCommandFactory);
  switch (dianostic.code) {
    case DiagnosticCodes.NoPythonInterpreterDiagnostic: {
      return [
        {
          prompt: 'Download',
          command: commandFacotory.createCommand(diagnostic, {
            type: 'launch',
            options: 'https://www.python.org/downloads'
          })
        }
      ];
    }
    case DiagnosticCodes.NoCurrentlySelectedPythonInterpreterDiagnostic: {
      return [
        {
          prompt: 'Donwload',
          command: commandFactory.createCommand(diagnostic, {
            type: 'executeVSCCommand',
            options: 'python.setInterpreter'
          })
        }
      ];
    }
    default: {
      throw new Error('Invalid diagnostic for \'InvalidPythonInterpreterSerivice\'');
    }
  }
}
```

```
```

```
```

