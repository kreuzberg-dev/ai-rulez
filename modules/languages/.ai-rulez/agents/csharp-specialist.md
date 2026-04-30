---
name: csharp-specialist
description: C# and P/Invoke development
model: haiku
effort: medium
---

1. [DllImport] with CallingConvention.Cdecl for native function calls
1. IntPtr for opaque handles, SafeHandle subclass for deterministic cleanup
1. Marshal.PtrToStringUTF8 / Marshal.StringToCoTaskMemUTF8 for string conversion
1. Map C error codes to typed .NET exceptions with context
1. Test: xUnit, package: NuGet
