1. 导出方式：用 C 接口，避免名字修饰

```cpp
#ifdef MYLIB_EXPORTS
  #define MYLIB_API __declspec(dllexport)
#else
  #define MYLIB_API __declspec(dllimport)
#endif

#ifdef __cplusplus
extern "C" {
#endif

MYLIB_API int __stdcall MyLib_Init(void);

#ifdef __cplusplus
}
#endif
```
extern "C" 消除 C++ 名字修饰，保证不同编译器都能链接
显式指定调用约定（__stdcall 或 __cdecl），不要依赖默认值
也可用 .def 文件导出，并通过序号保持兼容

2. 不要直接导出 C++ 类

C++ 类布局、虚表、STL 实现在不同编译器甚至同一编译器不同版本间都不兼容。替代方案：

不透明句柄 + C 函数（最通用）：Handle* Create(); void Destroy(Handle*); int DoWork(Handle*, ...)
纯虚接口类（COM 风格）：只含纯虚函数、无数据成员、通过工厂函数创建，虚表布局相对稳定

3. 接口参数只用 POD 类型

用 int32_t、uint64_t、double、const char*、原始指针 + 长度，不用 std::string、std::vector、std::shared_ptr 等
结构体用 #pragma pack 显式指定对齐，并加 size/version 字段便于扩展
字符串明确编码（UTF-8 或 UTF-16），明确谁负责缓冲区

4. 内存谁分配谁释放

DLL 和调用方可能使用不同的 CRT 堆（/MD 与 /MT 混用是典型事故）。规则：DLL 内分配的内存必须由 DLL 提供的函数释放，提供成对的 Create/Destroy、Alloc/Free。