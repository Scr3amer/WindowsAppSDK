FileSavePicker Class
===

# Definition

Namespace: [Microsoft.Windows.Storage.Pickers](./Microsoft.Windows.Storage.Pickers.md)

Represents a UI element that lets the user choose a file to save.

# Constructor

C#

```csharp
using Microsoft.Windows.Storage.Pickers;

var savePicker = new FileSavePicker(this.AppWindow.Id)
{
    // (Optional) specify the initial location. If not defined, use system default:
    SuggestedStartLocation = PickerLocationId.DocumentsLibrary,
    
    // (Optional) specify the default file name. If not defined, use system default:
    SuggestedFileName = "My Document",

    // (Optional) specify the text displayed on commit button. If not defined, use system default:
    CommitButtonText = "Save Document",

    // (Optional) categorized extensions types. If not defined, use system default: All Files (*.*)
    FileTypeChoices = {
        { "Documents", new List<string> { ".txt", ".doc", ".docx" } }
    },

    // (Optional) specify the default file extension.
    DefaultFileExtension = ".txt",
};
```

C++

```cpp
#include <winrt/Microsoft.Windows.Storage.Pickers.h>
using namespace winrt::Microsoft::Windows::Storage::Pickers;

FileSavePicker savePicker(AppWindow().Id());

// (Optional) specify the initial location. If not defined, use system default:
savePicker.SuggestedStartLocation(PickerLocationId::DocumentsLibrary);

// (Optional) specify the default file name. If not defined, use system default:
savePicker.SuggestedFileName(L"NewDocument");

// (Optional) categorized extensions types. If not defined, use system default: All Files (*.*)
savePicker.FileTypeChoices().Insert(L"Text", winrt::single_threaded_vector<winrt::hstring>({ L".txt" }));

// (Optional) specify the default file extension.
savePicker.DefaultFileExtension(L".txt");
```

# Methods

## PickSaveFileAsync

Displays a UI element that allows the user to configure the file path to save.

### Returns
```
IAsyncOperation<Windows.Storage.StorageFile>
```
Return null if the file dialog was cancelled or closed without selection.

### Examples

C#

```csharp
using Microsoft.Windows.Storage.Pickers;

var savePicker = new FileSavePicker(this.AppWindow.Id);
var file = await savePicker.PickSaveFileAsync();
if (file is not null)
{
    System.IO.File.WriteAllText(file.Path, "Hello world.");
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

FileSavePicker savePicker(AppWindow().Id());
StorageFile file& = co_await savePicker.PickSaveFileAsync();
if (file != nullptr)
{
    std::ofstream outFile(file.Path().c_str());
    outFile << "Hello world.";
    outFile.close();
}
else
{
    // error handling.
}
```
