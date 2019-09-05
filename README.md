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







```

```
```

```
```

