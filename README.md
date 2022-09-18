# C# Process Memory Read/Write
API 호출을 통해 프로세스 메모리에 접근하여 데이터를 읽거나 수정할 수 있는 소스 코드입니다.

# 사용법
```C#
ProcessMemoryManager pmm = new ProcessMemoryManager(7760);

// Read
//pmm.ReadMemory(0x5E366, new IntPtr(0x400000), out byte[] data, 2, out int bytesRead);
pmm.ReadMemory<short>(0x5E366, new IntPtr(0x400000), out byte[] data, out int bytesRead);
Console.WriteLine(BitConverter.ToInt16(data, 0));

// Write
pmm.WriteMemory(0x5E366, new IntPtr(0x400000), BitConverter.GetBytes((short)10), out int bytesWritten);

// Convert Data
Console.WriteLine(BitConverter.ToInt16(data, 0));
```
