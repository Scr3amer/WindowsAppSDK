FileOpenPicker Class
===

# Definition

Namespace: [Microsoft.Windows.Storage.Pickers](./Microsoft.Windows.Storage.Pickers.md)

Represents a UI element that lets the user choose and open files.

Supports specifying the initial location and text on commit button. 

# Constructor

C#

```csharp
using Microsoft.Windows.Storage.Pickers;

var openPicker = new FileOpenPicker(this.AppWindow.Id)
{
    // (Optional) specify the initial location. If not defined, use system default:
    SuggestedStartLocation = PickerLocationId.DocumentsLibrary,
    
    // (Optional) special the text diaplayed on commit button. If not defined, use system default:
    CommitButtonText = "Choose selected files",

    // (Optional) limit file extensions filters. If not defined, default to all (*.*)
    FileTypeFilter = { ".txt", ".pdf", ".doc", ".docx" },
};
```

C++

```cpp
#include <winrt/Microsoft.Windows.Storage.Pickers.h>
using namespace winrt::Microsoft::Windows::Storage::Pickers;

FileOpenPicker openPicker(AppWindow().Id());

openPicker.SuggestedStartLocation(PickerLocationId::DocumentsLibrary);
openPicker.CommitButtonText(L"Choose selected files");
openPicker.FileTypeFilter().Append(L".txt");
openPicker.FileTypeFilter().Append(L".pdf");
openPicker.FileTypeFilter().Append(L".doc");
openPicker.FileTypeFilter().Append(L".docx");
```

# Methods

## PickSingleFilesAsync

Displays a UI element that allows user to choose and open one file.

### Returns
```
IAsyncOperation<Windows.Storage.StorageFile>
```
Return null if the file dialog was cancelled or closed without selection.

### Examples

C#

```csharp
using Microsoft.Windows.Storage.Pickers;

var openPicker = new FileOpenPicker(this.AppWindow.Id);

var file = await openPicker.PickSingleFileAsync();
if (file is not null)
{
    var content = System.IO.File.ReadAllText(file.Path);
}
else
{
    // error handling.
}
```

C++
```cpp
#include <winrt/Microsoft.Windows.Storage.Pickers.h>
#include <fstream>
#include <string>
using namespace winrt::Microsoft::Windows::Storage::Pickers;

FileOpenPicker openPicker(AppWindow().Id());
auto& file = co_await openPicker.PickSingleFileAsync();
if (file != nullptr)
{
    std::ifstream fileReader(file.Path().c_str());
    std::string text((std::istreambuf_iterator<char>(fileReader)), std::istreambuf_iterator<char>());
    winrt::hstring hText = winrt::to_hstring(text);
}
else
{
    // error handling.
}
```

## PickMultipleFilesAsync

Displays a UI element that allows user to choose and open multiple files.

### Returns
```
IAsyncOperation<IReadOnlyList<Windows.Storage.StorageFile>>
```
Return an empty list (Count = 0) if the file dialog's cancelled or closed.

### Examples

C#

```csharp
using Microsoft.Windows.Storage.Pickers;

var openPicker = new FileOpenPicker(this.AppWindow.Id);

var files = await openPicker.PickMultipleFilesAsync();
if (files is not null)
{
    var pickedFilePaths = files.Select(f => f.Path);
    foreach (var path in pickedFilePaths)
    {
        var content = System.IO.File.ReadAllText(path);
    }
}
else
{
    // error handling.
}
```

C++
```cpp
#include <winrt/Microsoft.Windows.Storage.Pickers.h>
#include <fstream>
#include <string>
using namespace winrt::Microsoft::Windows::Storage::Pickers;

FileOpenPicker openPicker(AppWindow().Id());
auto& files = co_await openPicker.PickMultipleFilesAsync();
if (files.Size() > 0)
{
    for (auto const& file : files)
    {
        std::ifstream fileReader(file.Path().c_str());
        std::string text((std::istreambuf_iterator<char>(fileReader)), std::istreambuf_iterator<char>());
        winrt::hstring hText = winrt::to_hstring(text);
    }
}
else
{
    // error handling.
}
```
