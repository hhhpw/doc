## workbox

[参考文档](https://blog.csdn.net/jiezaizone/article/details/105815491)

[参考文档](https://blog.csdn.net/yelin042/article/details/79837745?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.pc_relevant_paycolumn_v3&spm=1001.2101.3001.4242.1&utm_relevant_index=3)


- workbox.skipWaiting()

为了在页面更新的过程中，新的sw脚本能立刻激活和生效

- workbox.clientClaim()
  
在新安装的sw后，打开页面都会使用版本更新的缓存




- 一个demo. 缓存所有js文件

```js
  workbox.routing.registerRoute(
        new RegExp('.*\.js'),
        new workbox.strategies.NetworkFirst()
    );

  workbox.routing.registerRoute(
     new RegExp('\\.js$'),
     jsHandler // 
   );
   
   workbox.routing.registerRoute(
     new RegExp('\\.css$'),
     cssHandler
   );
   
   workbox.routing.registerRoute(
     new RegExp('/blog/\\d{4}/\\d{2}/.+'),
     handler
   );

```

- workbox.routing

serveice worker可以拦截网页的网络请求，它可以把从网络请求中缓存的或者在serveic worker众请求获取的内容返回给浏览器，
也就是说浏览器不会再直接与服务器交互，而是由service worker代理。
Workerbox.routing是一个模块，可以容易地将这些请求“路由”映射到不同的响应，并提供设置相应策略的方法。

- workbox.precaching.preacheAndRoute 
  预缓存静态资源

```js

workbox.precaching.preacheAndRoute([
    '/styles/index.0c9a31.css',
    '/scripts/main.0d5770.js',
    {
        url: '/index.html',
        revision: '383676'
    },
]);
```


- 缓存策略
  当workbox捕获到一条已存于缓存中的请求时，以何种方式获取他的返回（网络请求/缓存）


```js
workbox.routing.registerRoute(
       new RegExp('.*\.js'),
       workbox.strategies.staleWhileRevalidate()
   );

```

1、StaleWhileRevalidate



StaleWhileRevalidate模式允许你使用缓存的内容尽快响应请求（如果可用），如果未缓存，则回退到网络请求。
然后，网络请求用于更新缓存。这是一种相当常见的策略，适合更新频率很高但重要性要求不高（至少允许一次缓存
读取）的内容。在有缓存的情况下，该模式保证了客户端第一时间拿到数据的同时，也会去请求网络资源更新缓存，
保证下次客户端从缓存中读取的数据是最新的。因此该模式不能减轻后台服务器的访问压力，但却能给前端用户提供
快速响应的体验。



2、Cache First (缓存优先，缓存回落到网络)

如果缓存中有响应，则将使用缓存的响应来完成请求，根本不会使用网络。 如果没有缓存的响应，则将通过网络请求
来满足请求，并且将缓存响应，以便直接从缓存提供下一个请求。该模式可以在为前端提供快速响应的同时，减轻后
台服务器的访问压力。但是数据的时效性就需要开发者通过设置缓存过期时间或更改sw.js里面的修订标识来完成缓
存文件的更新。一般需要结合实际的业务场景来判断数据的时效性。



3、Network First (网络回落到缓存)

对于经常更新且关键（由业务判断出来）的请求，网络优先策略是理想的解决方案。 默认情况下，它将尝试从网络获
取最新请求。 如果请求成功，它会将响应放入缓存中。 如果网络无法返回响应，则将使用缓存响应。这意味着只要当
第一次请求成功时，service worker就会缓存一份请求结果。当后续重复请求时，缓存也会每次更新。若网络请求失
败时，只要缓存里有内容，就能让前端获取一个响应（从service worker的缓存里）。因此，此种模式提高了前端页
面的稳固性，即使在网络请求失败的情况下，页面也能从上次的缓存中读取到数据展示，而不是粗鲁的告诉用户网络请
求失败的一个响应。
————————————————
版权声明：本文为CSDN博主「我是杰仔zone」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/jiezaizone/article/details/105815491


4、Network Only

5、Cache Only

6、 自定义策略
```js
workbox.routing.registerRoute(
    ({url, event}) => {
        return {
            name: 'workbox',
            type: 'guide',
        };
    },
    ({url, event, params}) => {
        // 返回的结果是：A guide on workbox
        return new Response(
            `A ${params.type} on ${params.name}`
        );
    }
);

7、 第三方请求的缓存

```

- 配置策略


```js

  workbox.routing.registerRoute(
        new RegExp('.*\.js'),
        workbox.strategies.staleWhileRevalidate({
            cacheName: 'js-cache', // 缓存名字
             plugins: [
                new workbox.expiration.Plugin({
                    maxEntries: 60, // 最大条目限制为60条
                    maxAgeSeconds: 30 * 24 * 60 * 60, // 过期期限30天
                }),
                new workbox.cacheableResponse.Plugin({
                    // 缓存状态代码为“200”或“0”的所有请求。
                    statuses: [200, 0],
                    // 处理符合正则的请求URL的响应时，请查看名为X-Is-Cacheable的标头（将由服务器添加到响应中）。
                    // 如果该标头存在，并且如果它设置为值'true'，则可以缓存响应
                    headers: {
                        'X-Is-Cacheable': 'true',
                    }
                    /**
                     * 注意: 如同时设置了上述条件statuses和headers，则需要两者都满足条件才视为可加入缓存
                     */
                })
            ]
        })
    );

```

