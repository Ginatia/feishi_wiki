
## 异步方法

```csharp
static async Task doWork()
{
	await Task.Delay(1000);
}
```

## 异步 Main

在 `Main` 方法中使用 `await` 运算符时，必须用 `async` 修饰符标记它。


```csharp
using System;
using System.Diagnostics;

namespace ConsoleApp1
{
    internal class Program
    {
        static async Task doWork()
        {
            await Task.Delay(1000);
        }

        static async Task Main(string[] args)
        {
            var sw = new Stopwatch();
            sw.Start();

            Task t1 = doWork();
            Task t2 = doWork();
            Task t3 = doWork();

            await Task.WhenAll(t1, t2, t3);

            Console.WriteLine("Finished");

            sw.Stop();

            var e = sw.ElapsedMilliseconds;

            Console.WriteLine($"elapsed: {e} ms");
        }
    }
}

```

## CPU 密集

```csharp
using System;
using System.Diagnostics;
using System.Text;

namespace ConsoleApp1
{
    internal class Program
    {
        static async Task<int> f1(int n)
        {
            var text = new StringBuilder();

            for(int i = 0; i < n; i++)
            {
                text.Append(i.ToString());
            }
            Console.WriteLine("F1 Finished");

            return await Task.FromResult(text.Length);
        }

        static async Task Main(string[] args)
        {
            var tasks = new List<Task<int>>();

            tasks.Add(Task.Run(() => f1(100_000)));
            tasks.Add(Task.Run(() => f1(200_000)));

            await Task.WhenAll(tasks);

            Console.WriteLine(await tasks[0]);
            Console.WriteLine(await tasks[1]);
        }
    }
}
```

## 异步网络请求

```csharp
using System;
using System.Diagnostics;
using System.Text;
using System.Text.RegularExpressions;

namespace ConsoleApp1
{
    internal class Program
    {
        static async Task Main(string[] args)
        {
            string[] urls = {
            "http://webcode.me",
            "http://httpbin.org",
            "https://github.com"
            };
            var rx = new Regex(@"<title>\s*(.+?)\s*</title>",RegexOptions.Compiled);
            
            using var client = new HttpClient();
            var tasks = new List<Task<string>>();

            foreach(var url in urls)
            {
                tasks.Add(client.GetStringAsync(url));
            }

            Task.WaitAll(tasks);

            var data = new List<string>();

            foreach (var task in tasks)
            {
                data.Add(await task);
            }

            foreach (var content in data)
            {
                var matches = rx.Matches(content);

                foreach (var match in matches)
                {
                    Console.WriteLine(match);
                }
            }
        }
    }
}

```

