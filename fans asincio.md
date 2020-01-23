```python
import turtle
joe = turtle.Turtle()


joe.forward(100)
joe.left(120)
joe.forward(100)
joe.left(120)
joe.forward(100)
joe.left(120)



screen = turtle.Screen()
screen.listen()
screen.mainloop()

exit()
```

***


```python
import queue
 
def task(name, work_queue):
    if work_queue.empty():
        print(f"Task {name} nothing to do")
    else:
        while not work_queue.empty():
            count = work_queue.get()
            total = 0
            print(f"Task {name} running")
            for x in range(count):
                total += 1
            print(f"Task {name} total: {total}")

def main():
    """
    Это основная точка входа в программу
    """
    # Создание очереди работы
    work_queue = queue.Queue()
 
    # Помещение работы в очередь
    for work in [15, 10, 5, 2]:
        work_queue.put(work)
 
    # Создание нескольких синхронных задач
    tasks = [(task, "One", work_queue), (task, "Two", work_queue)]
 
    # Запуск задач
    for t, n, q in tasks:
        t(n, q)

if __name__ == "__main__":
    main()
```

    Task One running
    Task One total: 15
    Task One running
    Task One total: 10
    Task One running
    Task One total: 5
    Task One running
    Task One total: 2
    Task Two nothing to do
    


```python
import queue
 

def task(name, queue):
    while not queue.empty():
        count = queue.get()
        total = 0
        print(f"Task {name} running")
        for x in range(count):
            total += 1
            yield
        print(f"Task {name} total: {total}")
 
 
def main():
    """
    Это основная точка входа в программу
    """
    # Создание очереди работы
    work_queue = queue.Queue()
 
    # Размещение работы в очереди
    for work in [15, 10, 5, 2]:
        work_queue.put(work)
 
    # Создание задач
    tasks = [task("One", work_queue), task("Two", work_queue)]
 
    # Запуск задач
    done = False
    while not done:
        for t in tasks:
            try:
                next(t)
            except StopIteration:
                tasks.remove(t)
            if len(tasks) == 0:
                done = True
 
 
if __name__ == "__main__":
    main()
```

    Task One running
    Task Two running
    Task Two total: 10
    Task Two running
    Task One total: 15
    Task One running
    Task Two total: 5
    Task One total: 2
    


```python
import time
import queue
from codetiming import Timer
 

    
def task(name, queue):
    timer = Timer(text=f"Task {name} elapsed time: {{:.1f}}")
    while not queue.empty():
        delay = queue.get()
        print(f"Task {name} running")
        timer.start()
        time.sleep(delay)
        timer.stop()
        yield
 
 
def main():
    """
    Это основная точка входа в программу
    """
    # Создание очереди работы
    work_queue = queue.Queue()
 
    # Добавление работы в очередь
    for work in [15, 10, 5, 2]:
        work_queue.put(work)
 
    tasks = [task("One", work_queue), task("Two", work_queue)]
 
    # Запуск задач
    done = False
    with Timer(text="\nTotal elapsed time: {:.1f}"):
        while not done:
            for t in tasks:
                try:
                    next(t)
                except StopIteration:
                    tasks.remove(t)
                if len(tasks) == 0:
                    done = True

if __name__ == "__main__":
    main()
```

    Task One running
    Task One elapsed time: 15.0
    Task Two running
    Task Two elapsed time: 10.0
    Task One running
    Task One elapsed time: 5.0
    Task Two running
    Task Two elapsed time: 2.0
    
    Total elapsed time: 32.0
    


```python
import asyncio
from codetiming import Timer
 
async def task(name, work_queue):
    timer = Timer(text=f"Task {name} elapsed time: {{:.1f}}")
    while not work_queue.empty():
        delay = await work_queue.get()                                    # await
        print(f"Task {name} running")
        timer.start()
        await asyncio.sleep(delay)                                        # await
        timer.stop()
 
 
async def main():
    """
    Это главная точка входа для главной программы
    """
    # Создание очереди работы
    work_queue = asyncio.Queue()
 
    # Помещение работы в очередь
    for work in [15, 10, 5, 2]:
        await work_queue.put(work)                                        # await
 
    # Запуск задач
    with Timer(text="\nTotal elapsed time: {:.1f}"):
        await asyncio.gather(                                             # await
            asyncio.create_task(task("One", work_queue)),
            asyncio.create_task(task("Two", work_queue)),
        )
 
 
if __name__ == "__main__":
    await main()                                                          # await
    # asyncio.run(main())
```

    Task One running
    Task Two running
    Task Two elapsed time: 10.0
    Task Two running
    Task One elapsed time: 15.0
    Task One running
    Task Two elapsed time: 5.0
    Task One elapsed time: 2.0
    
    Total elapsed time: 17.0
    


```python
import queue
import requests
from codetiming import Timer
 

def task(name, work_queue):
    timer = Timer(text=f"Task {name} elapsed time: {{:.1f}}")
    with requests.Session() as session:
        while not work_queue.empty():
            url = work_queue.get()
            print(f"Task {name} getting URL: {url}")
            timer.start()
            session.get(url)
            timer.stop()
            yield
 
 
def main():
    """
    Это основная точка входа в программу
    """
    # Создание очереди работы
    work_queue = queue.Queue()
 
    # Помещение работы в очередь
    for url in [
        "http://google.com",
        "http://yahoo.com",
        "http://linkedin.com",
        "http://apple.com",
        "http://microsoft.com",
        "http://facebook.com",
        "http://twitter.com",
    ]:
        work_queue.put(url)
 
    tasks = [task("One", work_queue), task("Two", work_queue)]
 
    # Запуск задачи
    done = False
    with Timer(text="\nTotal elapsed time: {:.1f}"):
        while not done:
            for t in tasks:
                try:
                    next(t)
                except StopIteration:
                    tasks.remove(t)
                if len(tasks) == 0:
                    done = True
 
 
if __name__ == "__main__":
    main()
```

    Task One getting URL: http://google.com
    Task One elapsed time: 0.2
    Task Two getting URL: http://yahoo.com
    Task Two elapsed time: 1.6
    Task One getting URL: http://linkedin.com
    Task One elapsed time: 0.8
    Task Two getting URL: http://apple.com
    Task Two elapsed time: 0.3
    Task One getting URL: http://microsoft.com
    Task One elapsed time: 0.7
    Task Two getting URL: http://facebook.com
    Task Two elapsed time: 0.4
    Task One getting URL: http://twitter.com
    Task One elapsed time: 0.6
    
    Total elapsed time: 4.6
    


```python
import asyncio
import aiohttp
from codetiming import Timer
 

async def task(name, work_queue):
    timer = Timer(text=f"Task {name} elapsed time: {{:.1f}}")
    async with aiohttp.ClientSession() as session:
        while not work_queue.empty():
            url = await work_queue.get()                                # await
            print(f"Task {name} getting URL: {url}")
            timer.start()
            async with session.get(url) as response:
                await response.text()                                   # await
            timer.stop()
 
 
async def main():
    """
    Это основная точка входа в программу
    """
    # Создание очереди работы
    work_queue = asyncio.Queue()
 
    # Помещение работы в очередь
    for url in [
        "http://google.com",
        "http://yahoo.com",
        "http://linkedin.com",
        "http://apple.com",
        "http://microsoft.com",
        "http://facebook.com",
        "http://twitter.com",
    ]:
        await work_queue.put(url)                                      # await
 
    # Запуск задач
    with Timer(text="\nTotal elapsed time: {:.1f}"):
        await asyncio.gather(                                          # await
            asyncio.create_task(task("One", work_queue)),
            asyncio.create_task(task("Two", work_queue)),
        )
 
 
if __name__ == "__main__":
    await main()                                                      # await
    #asyncio.run(main())
```

    Task One getting URL: http://google.com
    Task Two getting URL: http://yahoo.com
    Task One elapsed time: 0.2
    Task One getting URL: http://linkedin.com
    Task One elapsed time: 0.8
    Task One getting URL: http://apple.com
    Task One elapsed time: 0.3
    Task One getting URL: http://microsoft.com
    Task Two elapsed time: 1.6
    Task Two getting URL: http://facebook.com
    Task One elapsed time: 0.6
    Task One getting URL: http://twitter.com
    Task Two elapsed time: 0.4
    Task One elapsed time: 0.6
    
    Total elapsed time: 2.5
    

***


```python
# vvedenie
import asyncio


async def spider(site_name):
    for page in range(4):
        await asyncio.sleep(1)
        print(f"site name is {site_name} page is {page}")

        
async def main():
    tasks = []
    for name in ["Blog", "News", "Forum"]:
        cf = spider(name)
        t = asyncio.create_task(cf)  # ensure_future
        tasks.append(t)
    await asyncio.gather(*tasks)


await main()
```

    site name is Blog page is 0
    site name is News page is 0
    site name is Forum page is 0
    site name is Blog page is 1
    site name is News page is 1
    site name is Forum page is 1
    site name is Blog page is 2
    site name is News page is 2
    site name is Forum page is 2
    site name is Blog page is 3
    site name is News page is 3
    site name is Forum page is 3
    


```python
# call_later
# call_at
import asyncio
from time import time

def loader(url, t):
    print(f"load {url} time at {time() - t}")


async def spider(site_name):
    for page in range(4):
        await asyncio.sleep(1)
        print(f"site name is {site_name} page is {page}")

        
async def main():
    tasks = []
    for name in ["Blog", "News", "Forum"]:
        cf = spider(name)
        t = asyncio.create_task(cf)  # ensure_future
        tasks.append(t)
    await asyncio.gather(*tasks)



start_time = time()
loop = asyncio.get_running_loop()

loop_time = loop.time()
loop.call_later(2.5, loader, "loop", start_time)
loop.call_at(loop_time + 2.5, loader, "loop", start_time)
await main()
loop.call_soon(loader, "loop", start_time);
```

    site name is Blog page is 0
    site name is News page is 0
    site name is Forum page is 0
    site name is Blog page is 1
    site name is News page is 1
    site name is Forum page is 1
    load loop time at 2.5145680904388428
    load loop time at 2.5145680904388428
    site name is Blog page is 2
    site name is News page is 2
    site name is Forum page is 2
    site name is Blog page is 3
    site name is News page is 3
    site name is Forum page is 3
    load loop time at 4.015396356582642
    


```python
# delegirovanie
import asyncio


async def get_pages(site_name):
    await asyncio.sleep(0.5)
    print(f"Get page {site_name}")
    return range(1, 4)


async def get_page_data(site_name, page):
    await asyncio.sleep(1)
    return f"Data from page {page}  {site_name}"


async def spider(site_name):
    pages = await get_pages(site_name)
    for page in pages:
        data = await get_page_data(site_name, page)
        print(data)
        
        
async def main():
    tasks = []
    for name in ["Blog", "News", "Forum"]:
        cf = spider(name)
        t = asyncio.create_task(cf)  # ensure_future
        tasks.append(t)
    await asyncio.gather(*tasks)


await main()
```

    Get page Blog
    Get page News
    Get page Forum
    Data from page 1  Blog
    Data from page 1  News
    Data from page 1  Forum
    Data from page 2  Blog
    Data from page 2  News
    Data from page 2  Forum
    Data from page 3  Blog
    Data from page 3  News
    Data from page 3  Forum
    


```python
# return
import asyncio


async def get_pages(site_name):
    await asyncio.sleep(0.5)
    print(f"Get page {site_name}")
    return range(1, 4)


async def get_page_data(site_name, page):
    await asyncio.sleep(1)
    return f"Data from page {page}  {site_name}"


async def spider(site_name):
    pages = await get_pages(site_name)
    all_data = []
    for page in pages:
        data = await get_page_data(site_name, page)
        all_data.append(data)
    return all_data
        
        
async def main():
    tasks = []
    for name in ["Blog", "News", "Forum"]:
        cf = spider(name)
        t = asyncio.create_task(cf)  # ensure_future
        tasks.append(t)
    data = await asyncio.gather(*tasks)
    return data

data = await main()
print(data)
```

    Get page Blog
    Get page News
    Get page Forum
    [['Data from page 1  Blog', 'Data from page 2  Blog', 'Data from page 3  Blog'], ['Data from page 1  News', 'Data from page 2  News', 'Data from page 3  News'], ['Data from page 1  Forum', 'Data from page 2  Forum', 'Data from page 3  Forum']]
    


```python
# callback
import asyncio
import functools


def upper_data(future):
    print(future.result().upper())

    
def convert_data(method, future):
    result = future.result()
    print(getattr(result, method)())
    

async def get_pages(site_name):
    await asyncio.sleep(0.5)
    print(f"Get page {site_name}")
    return range(1, 4)


async def get_page_data(site_name, page, future):
    await asyncio.sleep(1)
    future.set_result(f"Data from page {page}  {site_name}")


async def spider(site_name):
    pages = await get_pages(site_name)
    for page in pages:
        future = asyncio.Future()
        future.add_done_callback(upper_data)
        future.add_done_callback(functools.partial(convert_data, "lower"))
        await get_page_data(site_name, page, future)
        
        
async def main():
    tasks = []
    for name in ["Blog", "News", "Forum"]:
        cf = spider(name)
        t = asyncio.create_task(cf)  # ensure_future
        tasks.append(t)
    await asyncio.gather(*tasks)


await main()
```

    Get page Blog
    Get page News
    Get page Forum
    DATA FROM PAGE 1  BLOG
    data from page 1  blog
    DATA FROM PAGE 1  NEWS
    data from page 1  news
    DATA FROM PAGE 1  FORUM
    data from page 1  forum
    DATA FROM PAGE 2  BLOG
    data from page 2  blog
    DATA FROM PAGE 2  NEWS
    data from page 2  news
    DATA FROM PAGE 2  FORUM
    data from page 2  forum
    DATA FROM PAGE 3  BLOG
    data from page 3  blog
    DATA FROM PAGE 3  NEWS
    data from page 3  news
    DATA FROM PAGE 3  FORUM
    data from page 3  forum
    


```python
#as_completed
import asyncio


async def get_pages(site_name):
    await asyncio.sleep(0.5)
    print(f"Get page {site_name}")
    return range(1, 4)


async def get_page_data(site_name, page):
    await asyncio.sleep(1)
    return f"Data from page {page}  {site_name}"


async def spider(site_name):
    pages = await get_pages(site_name)
    copages = []
    for page in pages:
        copages.append(get_page_data(site_name, page))
        
    for page in asyncio.as_completed(copages):
        data = await page
        print(data)
        
        
async def main():
    tasks = []
    for name in ["Blog", "News", "Forum"]:
        cf = spider(name)
        t = asyncio.create_task(cf)  # ensure_future
        tasks.append(t)
    await asyncio.gather(*tasks)


await main()
```

    Get page Blog
    Get page News
    Get page Forum
    Data from page 2  Blog
    Data from page 3  Blog
    Data from page 1  Blog
    Data from page 2  Forum
    Data from page 1  Forum
    Data from page 3  Forum
    Data from page 1  News
    Data from page 2  News
    Data from page 3  News
    
# Simple async server

```python
from aiohttp import web
import aioreloader
import argparse
import aiohttp_jinja2
import jinja2
import logging


parser = argparse.ArgumentParser(description="Best project")
parser.add_argument('--host', help='Host to listen', default='0.0.0.0')
parser.add_argument('--port', help='Port to accept connections', default='8080')
parser.add_argument('--reload', action='store_true', help='Autoreload code on change')

args = parser.parse_args()

if args.reload:
	aioreloader.start()

app = web.Application()
jinja_env = aiohttp_jinja2.setup(app, loader=jinja2.FileSystemLoader("./"), context_processors=[aiohttp_jinja2.request_processor])

async def handler(request):
    name = request.match_info.get('name', "Anonymous user")
    text = "Hello, " + name
    return web.Response(text=text)

class Index(web.View):
    @aiohttp_jinja2.template('template_name.html')
    async def get(self):
        return {'foo': 'boo'}

routes = (
		dict(method="GET", path="/", handler=handler, name="main"),
		dict(method='GET', path='/index', handler=Index, name='index'),
		dict(method="GET", path="/{name}", handler=handler, name="with_name"),
	)

for route in routes:
    app.router.add_route(**route)


web.run_app(app, host=args.host, port=args.port)
```
### template_name.html
```html
<h1>hello world</h1>

<a href="{{ app.router['index'].url_for() }}">Main page</a>
```
