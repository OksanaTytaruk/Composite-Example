# Composite Example

Цей репозиторій містить приклад патерну проектування "Компонувальник".

## Патерн Компонувальник (Composite)

Патерн "Компонувальник" дозволяє клієнтам працювати з індивідуальними об'єктами так само, як і з групами об'єктів.

### Приклад коду:

```csharp
// Компонент
public abstract class FileSystemComponent
{
    public string Name { get; private set; }

    public FileSystemComponent(string name)
    {
        Name = name;
    }

    public abstract void Display();
}

// Листок
public class File : FileSystemComponent
{
    public File(string name) : base(name) { }

    public override void Display()
    {
        Console.WriteLine($"Файл: {Name}");
    }
}

// Компонувальник
public class Directory : FileSystemComponent
{
    private List<FileSystemComponent> _components = new List<FileSystemComponent>();

    public Directory(string name) : base(name) { }

    public void Add(FileSystemComponent component)
    {
        _components.Add(component);
    }

    public void Remove(FileSystemComponent component)
    {
        _components.Remove(component);
    }

    public override void Display()
    {
        Console.WriteLine($"Каталог: {Name}");
        foreach (var component in _components)
        {
            component.Display();
        }
    }
}

// Клієнтський код:

class Program
{
    static void Main(string[] args)
    {
        Directory root = new Directory("Root");
        Directory documents = new Directory("Documents");
        Directory images = new Directory("Images");

        File file1 = new File("Resume.doc");
        File file2 = new File("Photo.jpg");

        root.Add(documents);
        root.Add(images);
        documents.Add(file1);
        images.Add(file2);

        root.Display();
    }
}
