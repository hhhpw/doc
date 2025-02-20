### 优势
开箱即用的脚手架，零配置，支持 TypeScript、ESLint、Tailwind.css 等常用技术选型，快速上手使用。
内置各种优化组件，如图片、字体、脚本、链接等。
SEO 友好，基于服务端组件和客户端组件进行客户端渲染和服务端渲染，自动进行静态、动态路由优化，支持流式传输。
基于文件的路由系统，支持布局、嵌套路由、加载状态、错误处理等高级路由功能，解决了复杂场景下的路由实现。
内置多种 CSS 支持，如 CSS Modules、Tailwind CSS 和 CSS-in-JS。

### 渲染模式
1. 静态生成（Static Generation，SSG）：在构建时生成 HTML 文件，适用于静态内容（如博客、营销网站等）。
使用 getStaticProps 或 getStaticPaths 实现。
2. 服务器端渲染（Server-Side Rendering，SSR）：每次请求时，服务器渲染页面并返回 HTML，适用于需要动态数据的页面。
使用 getServerSideProps 实现。
3. 客户端渲染（CSR）：客户端负责渲染页面，类似于单页应用（SPA）。
直接渲染组件，不需要数据获取函数。

### 水合  Hydration


水合（Hydration）是指在 客户端（浏览器）接管从 服务器端渲染（SSR）返回的 HTML 内容的过程。在 Next.js 中，水合是页面从服务器返回的 HTML 内容变得动态可交互的过程。

水合（Hydration）过程的工作原理：
服务器渲染： 当用户访问一个 Next.js 页面时，服务器首先根据请求生成 HTML 内容，这通常是通过 服务器端渲染（SSR） 或 静态生成（SSG） 来完成的。这个 HTML 页面会被发送到客户端，并包含一些基本的内容。

客户端加载： 一旦浏览器加载了这个 HTML 页面，它会从服务器请求 JavaScript 文件。这些 JavaScript 文件包含了 React 代码，以及用来将页面变为动态的所有代码。

React 接管： 客户端的 React 会接管这个已经渲染好的 HTML 内容，将它与 React 的虚拟 DOM（Virtual DOM）进行对比，确保客户端的 React 应用与从服务器端发送的 HTML 保持一致。这个过程就被称为“水合”。

交互性激活： 在水合完成之后，React 会处理事件监听、更新状态等工作，使页面具备交互能力。此时，用户可以开始与页面进行交互，例如点击按钮、表单输入等。

为什么水合是必要的？
页面渲染的速度：服务端渲染的 HTML 页面可以更快地展示给用户，从而减少首屏加载时间。用户不需要等到 JavaScript 被完全下载和执行后才能看到内容。

交互能力：水合使得静态的 HTML 页面变得动态可交互。仅通过 SSR 或 SSG 渲染的 HTML 页面本身是静态的，只有通过 React 的水合过程，客户端的交互才能生效。

##### Next.js 中的水合问题
虽然水合在提升用户体验和 SEO 方面非常重要，但它也可能带来一些挑战，特别是当服务器渲染的 HTML 内容与客户端 JavaScript 中的内容不一致时。以下是一些常见的水合相关问题：

1. 水合不匹配（Hydration Mismatch）： 如果服务端渲染的 HTML 和客户端生成的内容不一致，React 会抛出警告。这通常发生在 SSR 或 SSG 和客户端的初始渲染之间存在差异时。例如，服务端渲染的页面中某个部分的内容与客户端渲染时的内容不一致时，React 会报错并重新渲染页面。

解决方案：

- 确保服务器端渲染的输出与客户端渲染的输出一致。这包括所有的组件和数据都应该在服务器端准备好并渲染。
- 使用 useEffect 和 useLayoutEffect 钩子时，确保它们不会影响首次渲染的 HTML 输出。
- 避免在渲染过程中直接使用浏览器特定的 API（例如 window 或 document），因为这些 API 仅在客户端可用，服务器端无法访问它们。

2. 客户端和服务器端渲染差异： 由于 JavaScript 和浏览器的行为，可能导致服务器渲染和客户端渲染之间的一些差异。例如，使用 Math.random() 或获取当前日期时间等客户端计算时，服务端和客户端的计算结果可能不同。这样会导致渲染结果不一致。

解决方案：

- 尽量避免在组件的渲染过程中执行随机数、时间戳等客户端计算。
- 使用 useEffect 或 useLayoutEffect 来在客户端执行这类计算。

3.延迟水合： 在某些情况下，开发者可能希望延迟水合过程，直到客户端加载并运行 JavaScript 时才进行水合。这可以用于优化性能，尤其是在重度依赖 JavaScript 的页面中。

解决方案：

Next.js 默认在页面加载后进行水合，但你可以通过 next/script 组件来控制 JavaScript 的加载顺序和执行时机。

##### 水合的最佳实践：
- 确保一致性：确保在服务端和客户端渲染过程中渲染的 HTML 内容保持一致，避免因为客户端特定的计算（如随机数、时间等）导致差异。
- 使用 Suspense 和 Lazy Loading：在组件渲染中使用 React Suspense 和 React.lazy 进行懒加载可以有效避免不必要的客户端渲染，提高水合的效率。
- 优化首屏渲染：通过使用 静态生成（SSG） 或 增量静态再生成（ISR） 等方式，减少服务器渲染负担，优化首屏加载性能。
- 避免依赖浏览器特性：避免在组件的初始化渲染过程中直接访问浏览器的 API，避免客户端和服务器端行为不一致。
总结：

水合是 React 和 Next.js 中的一个重要概念，它是将服务器渲染的静态 HTML 页面与客户端的 React 应用进行结合的过程。通过水合，页面能在不牺牲首屏渲染速度和 SEO 的前提下，具备动态交互能力。要确保水合过程顺利，需要保持服务端和客户端渲染的内容一致，并遵循一些优化实践。

### 服务器端渲染  SSR

服务器端渲染 (SSR) 是一种网页渲染技术，它在服务器端生成完整的 HTML 页面，然后将页面发送给浏览器。与客户端渲染 (CSR) 不同，SSR 在服务器端完成页面渲染，浏览器只需解析和显示 HTML 页面即可。

SSR 的工作原理：当用户请求一个页面时，服务器会执行以下步骤：

获取页面数据和模板。
使用数据和模板生成完整的 HTML 页面。
将 HTML 页面发送给浏览器。
浏览器解析和显示 HTML 页面。

##### SSR 的优势：

- 更快的首屏加载速度： 由于浏览器无需等待 JavaScript 下载和执行，SSR 可以提供更快的首屏加载速度。
- 更好的 SEO： 搜索引擎可以更容易抓取和索引 SSR 生成的页面，从而提高网站的 SEO 排名。
- 更易于访问： SSR 生成的页面无需 JavaScript 即可访问，这对于禁用 JavaScript 的用户或低带宽连接的用户来说非常有用。

### 流式渲染 Stream Rendering
流式渲染（Stream Rendering）是一种逐步渲染和传输页面内容的技术，它可以在服务器端渲染（SSR）的基础上实现更高效的内容加载，特别适用于具有大量内容的页面。流式渲染可以在服务器渲染完部分内容后，立即将它们发送到客户端，而不是等待整个页面的渲染完成，这样可以提高页面的响应速度和用户体验。

在 Next.js 中，流式渲染的实现通常基于 React 18 的 Suspense 和 Concurrent Rendering 功能，它允许 React 通过流的方式逐步生成 HTML，并通过网络流传输给客户端。

流式渲染的工作原理
- 逐步生成内容：当请求到达服务器时，React 使用流式渲染逐步生成 HTML。它不会等到所有内容都完全渲染好再发送，而是将渲染好的部分内容立即通过流发送给客户端。

- 并行加载和渲染：通过 React Suspense 和 Concurrent Mode，可以使得不同的组件异步加载并并行渲染。这意味着，React 不会阻塞整个页面的渲染，可以在渲染过程中处理其他资源。

- 增量发送 HTML：流式渲染可以逐步地将 HTML 发送给客户端，客户端会尽早显示渲染好的内容，而不是等待整个页面渲染完成后再显示。

- 更快的首屏加载：通过流式渲染，浏览器可以尽早接收到 HTML 内容并开始显示，这对首屏渲染速度有极大的提升，尤其是对于包含大量内容的页面。

##### 流式渲染的优势

1. 更快的首屏加载：页面内容可以分批加载和显示，用户能更快地看到内容，尤其是在内容较多的页面中。
2. 并行渲染：可以并行加载多个组件，不会因为某个组件的渲染阻塞整个页面的渲染过程。
3. 减少用户等待时间：逐步加载和渲染页面的不同部分，避免页面全部加载完才呈现给用户，提升用户体验。

### RSC React Server Components

RSC 是 React 设计的一种新架构，这种方法旨在利用服务器和客户端环境的优势，实现用户体验、易于维护和高性能的三角平衡。

使用服务端组件有很多好处：

1. 数据获取：通常服务端环境（网络、性能等）更好，离数据源更近，在服务端获取数据会更快。通过减少数据加载时间以及客户端发出的请求数量来提高性能。
2. 安全：在服务端保留敏感数据和逻辑，不用担心暴露给客户端。
3. 缓存：服务端渲染的结果可以在后续的请求中复用，提高性能。
Bundle 大小：服务端组件的代码不会打包到 bundle 中，减少了 bundle 包的大小。
4. 初始页面加载和 FCP：服务端渲染生成 HTML，快速展示 UI。
5. Streaming：服务端组件可以将渲染工作拆分为 chunks，并在准备就绪时将它们流式传输到客户端。用户可以更早看到页面的部分内容，而不必等待整个页面渲染完毕
1. 第一个是 bundle size，将组件拆分为客户端组件和服务端组件后，服务端组件在服务端渲染即可，客户端只需要最后的渲染结果，所以服务端组件的依赖项不需要打包到客户端 bundle 中，这就减少了客户端 JS 的大小。

2. 第二个是局部渲染和水合，传统的 SSR 实现中，所有的组件代码都要下载到客户端以进行水合，但是在 RSC 中，因为明确进行了组件区分，所以可以做到只有客户端组件进行水合。在后续导航的时候，RSC 将组件的渲染放在服务端，并渲染成特殊格式的 RSC Payload，根据 RSC Payload，客户端可以进行局部渲染和更新，由此实现了状态的保持。
### Suspense

### 性能优化手段
1. 渲染策略，SSG渲染、流式渲染，选择合适的渲染策略
2. 减少JS大小。尽可能使用RSC
3. 优先使用内置组件
内置了Image组件实现了懒加载，图片优化等
Link组件实现了预加载等
4. 性能测试工具。@next/bundle-analyzer分析包的大小,压缩图片

### getStaticProps 和 getServerSideProps 的区别是什么？

- getStaticProps：在构建时执行，适用于静态站点生成（SSG）。页面的数据会在构建时被获取，并生成静态的 HTML 文件。对于不常变化的数据非常适用。
- getServerSideProps：在每次请求时执行，适用于服务器端渲染（SSR）。每次请求都会从服务器获取最新的数据并渲染页面，适用于需要动态数据的页面。

### 什么是静态页面生成（SSG）和动态页面生成（SSR）？它们的优缺点是什么？
解答：
- 静态页面生成（SSG）：在构建时生成 HTML 页面。每次请求时直接返回构建好的页面。
优点：页面加载更快，SEO 更好，服务器负担小。
缺点：无法动态获取每个请求的数据，适合静态内容。
- 动态页面生成（SSR）：每次请求时在服务器上生成页面并返回给客户端。
优点：页面数据实时获取，适用于需要动态内容的页面。
缺点：服务器负担较重，响应时间较长。

### 增量静态再生 ISR

增量静态再生（Incremental Static Regeneration，简称 ISR） 是 Next.js 提供的一个强大功能，允许你在不重新构建整个网站的情况下，更新和重新生成静态页面内容。它使得你可以为你的应用提供 静态页面生成（SSG） 的性能优势，同时还能应对动态内容的变化需求。通过 ISR，页面可以在静态生成后定期根据设置的时间间隔进行重新生成，从而确保页面内容始终是最新的。

- 如何工作
ISR 结合了静态生成（SSG）和服务器端渲染（SSR）的优点。在 Next.js 中，静态页面通常在构建时生成，但是有时需要页面动态更新（例如，数据内容经常变化）。ISR 使得你可以：

1. 在构建时生成静态页面。
2. 在客户端请求时，如果页面已经被缓存，可以直接返回静态内容。
3. 在页面请求之后，Next.js 会在后台重新生成该页面（例如，在 10 秒后），并在下一次请求时返回最新的内容。

**在 Next.js 中启用 ISR 非常简单。你只需要在 getStaticProps 中使用 revalidate 参数，它表示页面更新的间隔时间（单位是秒）。当页面的请求间隔超过这个时间，Next.js 会重新生成该页面。**

#### getServerSideProps、getStaticProps、getStaticPaths

- getServerSideProps 用于 服务器端渲染（SSR），每次请求都会调用该函数，从服务器动态获取数据并生成页面。这意味着页面内容在每次请求时都会重新计算，适用于需要根据请求参数（如用户身份、查询条件等）生成动态内容的页面。

- getStaticProps 用于 静态生成（SSG） 页面，在构建时获取数据并生成静态页面。它适用于那些内容不经常变化的页面，比如博客首页、产品页面等。静态生成的页面将在构建时生成一次，并在用户访问时直接返回预生成的 HTML 文件。

- getStaticPaths 用于 动态路由 页面（例如，/posts/[id]），告知 Next.js 在构建时需要生成哪些动态路由的页面。它通常与 getStaticProps 配合使用，用于生成多个静态页面，例如根据 API 数据生成动态的博客文章详情页。

