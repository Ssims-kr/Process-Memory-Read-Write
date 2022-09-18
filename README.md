# C# Process Memory Read/Write
API 호출을 통해 프로세스 메모리에 접근하여 데이터를 읽거나 수정할 수 있는 소스 코드입니다.

안 좋은 방향으로는 게임의 핵을 개발할 때 사용할 수 있고, 좋은 방향으로는 번역, 리버스 엔지니어링 공부 등으로 사용할 수 있습니다.

조금 더 간소화할 수 있지만 귀찮네요.

# 사용법
## Load(불러오기)
```C#
// [1] Process ID
int ProcessId = 1234; /* or You can get ID by Process class */
ProcessMemoryManager pmm = new ProcessMemoryManager(ProcessId);

// [2] Process Name
ProcessMemoryManager pmm = new ProcessMemoryManager("Process Name");
```
`Process` 클래스를 통해 Id 또는 프로세스의 이름을 얻어올 수 있습니다. 얻은 값을 생성자 인자로 넘겨주시면 됩니다.

## Read(읽기)
```C#
// Read
int Offset = 0x5E5E5;
int ImageBase = 0x400000;

pmm.ReadMemory<short>(Offset, new IntPtr(ImageBase), out byte[] data, out int bytesRead);
```
Offset과 ImageBase의 값을 인자로 넘깁니다. 실제 주소는 0x45E5E5가 되겠죠? 읽어온 데이터 값은 data에 담깁니다.

## Write(쓰기)
```C#
// Write
int Offset = 0x5E5E5;
int ImageBase = 0x400000;

// [1]
byte[] writeData = new byte[2] { 0x00, 0x01 };
pmm.WriteMemory(Offset, new IntPtr(ImageBase), writeData, out int bytesWritten);

// [2]
pmm.WriteMemory(Offset, new IntPtr(ImageBase), BitConverter.GetBytes((short)255), out int bytesWritten);
```
프로세스 메모리에 데이터를 입력합니다. 프로세스 메모리는 보호되기 때문에, 코드 내부에는 읽기/쓰기가 가능하도록 API를 호출하고 있습니다.

## Convert Data
```C#
// Convert
BitConverter.ToInt16(data);
BitConverter.ToInt32(data);
// ...
```
`ReadMemory`로 부터 읽어온 데이터를 `BitConveter` 클래스를 통해 변환하여 값을 얻을 수 있습니다.

## Check
```C#
if (pmm.ReadMemory<short>(Offset, new IntPtr(ImageBase), out byte[] data, out int bytesRead) == false) {
    // Shit
}
```
