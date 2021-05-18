



# Cloud Native :cloud: Wasm Day :spider_web: (4th May 2021) - Part II

<hr>

## Lightning Talks

### Migrating from JavaScript to WebAssembly (in the browser)

1. **Speaker**: Corentin Godeau is a Software Engineer from [Lumen Technologies](https://www.lumen.com/en-us/home.html) (formerly CenturyLink), a tech company providing infrastructures and platforms for companies.

2. [CDN Mesh Delivery](https://www.lumen.com/en-us/networking/cdn/mesh-delivery.html), uses P2P technology for efficient video delivery. Developed for the <u>web</u>.

3. Migrated CDN Mesh Delivery from <u>JavaScript --> Wasm via C++</u>.

   * Speed. Algorithms were compute-intensive.
   * Portability. Single code across variety of platforms.
   * Exploit hardware capabilities of native platforms.

4. Differences between <u>JS Vs. Wasm</u> influenced their code design.

   | JavaScript       | Wasm                 |
   | ---------------- | -------------------- |
   | High-level       | Low-level            |
   | Asynchronous     | Synchronous          |
   | Own environment  | Called from JS (web) |
   | Event loop-based | Stack-based          |

5. **Limitations of Wasm Vs. Native compilation**: 

   * <u>Multi-threading</u> weakly supported
   * <u>Asynchronous programming</u> makes the binary bigger and slower
   * <u>Non-preemptive</u>: code cannot resume if interrupted (to run other processes).

6. **Advice**

   * <u>Architecture</u>: Design APIs that are platform-agnostic but write wasm module implementations specific to a platform (web, native, common).
   * <u>Asynchronous design</u>: Design APIs that plan on eventually moving into asynchronous mode. It is round the corner.
   * <u>Actor model</u>: Design modules based on the Actor model. They adapt better to threading in different platforms.
   * <u>Debugging</u>: Perform debugging on native platforms. Debugging on Chrome isn't reliable.
   * <u>Emscripten tools</u>: `-Oz` for optimizing code size. `--profiling` for profiling on Chrome. `--memoryprofiler` profiling memory. `--closure 1` runs the Closure compiler to help reduce size.

7. Useful **boilerplate codes** for Emscripten: https://github.com/streamroot/web-native-common-samples

### Building web services for a future based on WebAssembly

1. **Speaker**: Connor Hicks is a Software Developer at [1Password](https://1password.com/), a password manager. He maintains [Suborbital](https://suborbital.dev/), a collection of open-source projects on server-side WebAssembly.
2. **Suborbital projects** include—
   * [Reactr](https://github.com/suborbital/reactr), a function schedule, manages, and runs Wasm modules.
   * [Grav](https://github.com/suborbital/grav), a messaging mesh that allows Go apps to communicate asynchronously.
   * [Vektor](https://github.com/suborbital/vektor), a HTTP API framework that integrates with Grav and Reactr.
   * [Atmo](https://github.com/suborbital/atmo)  = Reactr + Grav + Vektor. Provides APIs and server-side runtime to build cloud-native applications with Wasm.
3. **Atmo**
   * <u>Directive</u>: declarative file that describes the application. It lists all the endpoints, get files/repos/modules, and the logic to run them.
   * Inside the repository of your runnable module, you can import <u>suborbital APIs</u> that gives you access to the host (WASI).
   * <u>Languages supported</u>: Swift and Rust.
   * <u>Demo</u>: https://github.com/cohix/telescope

### Build trusted cloud apps with Wasm: WebAssembly Micro Runtime is ready

1. **Speaker**: Xin Wang is a Software Engineer at Intel.
2. Trusted applications are important to— 1) owners of the application (protecting apps from malicious agents), 2) cloud service providers (app security on the cloud; protecting host from malicious apps), and 3) data owners (data security).
3. **Solution**: build wasm modules on [Intel SGX](https://software.intel.com/content/www/us/en/develop/topics/software-guard-extensions.html) for double protection.
   * <u>Trust Execution Environment (TEE)</u>: provides hardware isolation. Protection from external attacks.
   * <u>Software Guard Extension (SGX)</u>: supports all Ice Lake Scalable processors.
   * <u>Wasm</u>: lightweight sandbox that supports scalable concurrent executions. Prevents internal attacks from malicious agents.
4. **WebAssembly Micro Runtime for SGX** ([WAMR](https://github.com/bytecodealliance/wasm-micro-runtime))
   * Small modules <100 KB.
   * Interpreter and AOT compiler for SGX.
   * Supports: Wasm SIMD, WASI on SGX.
   * **AI framework support**: TensorFlow, XNNPACK.
5. WAMR use cases—
   * Smart contracts.
   * Building confidential containers (inclavare containers).
   * Secured MPC.

### Storing WebAssembly Modules with Bindle

1. **Speaker**: Matt Butche (Principal Software Development Engineer) and Taylor Thomas (Senior Software Engineer) are Software Engineers at Azure, Microsoft. Matt has worked on Helm, Kubernetes, and Open Stack. Taylor has worked on Helm, Kubernetes, Krustlet, and Docker.

2. **Aggregate object storage**: store a collection of related objects as a single unit and shared metadata. 

3. **Bindle**: Unlike cloud storage and Docket's OCI Images, Bindle can capture complex object relationships. It is composed of 2 parts— 1) invoice (metadata: name, version, authors, list of parcels, group parcel, signature), and 2) parcels (objects: a blob).

   <img src="/Users/jeya/Documents/projects/learn/wasm/CNCF Cloud Wasm Day/images/bindle.png" alt="bindle" style="zoom:30%;" />

4. **Features and Groups**: link a file with all it's related files (e.g., dependencies in a project like *.html, *.css, and *.js.).

5. **Signature**: identify files intended by an author. No pollution from 3rd party. For provenance.

6. **Simple Example**: https://github.com/deislabs/bindle/blob/main/test/data/simple-invoice.toml

7. **Complex Example**: https://github.com/deislabs/bindle/blob/main/test/data/full-invoice.toml

8. **Example of Bindle on wasm files**: https://github.com/deislabs/bindle/blob/main/docs/webassembly.md#example-5-heavy-and-light-alternatives

   * <u>Heavy wasm</u>: run on resource rich environment.
   * <u>Light wasm</u>: run on resource poor environment.
   * Retrieve appropriate file according to environment.

<hr>

## Serverless WebAssembly for compute-intensive workloads

### Speaker

1. Robert Aboukhalil is a software engineer at [Invitae](https://www.invitae.com/en), a genetic testing company. He has a background in both computer science and bioinformatics. He is the author of the book [Level Up with WebAssembly](https://www.levelupwasm.com/).

### Wasm for data analysis on the browser

1. Great examples of compute-intensive data analysis being run on browser using Wasm—
   * [AutoCAD](https://madewithwebassembly.com/showcase/autocad/) ported its 30 year-old web app codebase to Wasm.
   * [eBay](https://tech.ebayinc.com/engineering/webassembly-at-ebay-a-real-world-use-case/) implemented its barcode scanner in Wasm.
   * [TensroFlow.js](https://blog.tensorflow.org/2020/03/introducing-webassembly-backend-for-tensorflow-js.html) provides a Wasm backend for machine learning.
   * [Genome Ribbon](https://genomeribbon.com/): compiled Samtools to Wasm to read BAM files.
2. **Benefits of browser compute**
   * Previous examples show that you can simply <u>reuse your existing codebase</u>.
   * Potential <u>performance improvement</u>.
   * Leverage <u>compute on edge</u>.
   * Keeping <u>local data</u> private.
3. What if data is too large and resides on the cloud? What if the edge doesn't have sufficient compute power? The browser
4. **Possible solutions**
   * Analysis on the Cloud (spot VMs, AWS Batch). Spin up VMs and run analysis there. Usually costly.
   * Serverless functions (Cloud functions/Lambda, Cloud Run/Fargate). Scaling done for you. Pricing on a per-use basis.
   * *Serverless Wasm functions* ([Cloudflare Workers](https://workers.cloudflare.com/), [Fastly Compute@Edge](https://www.fastly.com/products/edge-compute/serverless/)). Similar to traditional serverless functions with some additional benefits—1) fewer to no cold starts (because Wasm modules are much lighter than containers), 2) run the compiled code, from any language, on both front- and back-end.

### Serverless WebAssembly

1. **Application**: an API that generates simulated data (in FASTQ format) from a DNA sequencing experiment.
2. They compiled the C-based [WgSim](https://github.com/lh3/wgsim) into Wasm. The **API** was hosted in [Cloudflare Workers](https://workers.cloudflare.com/). API input (*n*: number of sequences; *chrom*: chromosome to simulate; *error*: error rate).
3. WgSim needs a <u>reference genome</u> upon which it simulates natural variation and sequencing errors. The reference genome was stored in an <u>S3 bucket</u> and <u>streamed in chunks</u>. This is because Cloudflare Workers have a RAM limit of 128 MB.
4. Why **Cloudflare Workers**? Scalable and pay-per-use like most serverless providers.
   * First-class <u>WebAssembly support</u>.
   * Nearly <u>non-existant cold starts</u>.
   * Powered by <u>V8</u>. So, you are technically still running this in a browser. Except the browser is hosted in a cloud. So, you use the exact same Wasm binary in both cloud and browser.
   * Provide <u>debugging tools</u>. Includes developer tools from Chrome. Except, these are not your browser developer tools but one for the instance in the cloud. When you edit your function, the changes takes effect into the debugging tools immediately without the need for re-deploying it.
   * <u>Key-value store at scale</u>. Store results as key-value pairs. Generated permalink can be shared with collaborators to share results.
   * <u>Cron jobs</u> for scheduled jobs. Results can be stored in key-value store.
5. **Weakness** of Cloudflare Workers.
   * 128 MB <u>RAM limit</u>.
   * 30 seconds <u>CPU limit</u>.

<hr>

## Machine Learning with Wasm (wasi-nn)

### 



<hr>

## WebAssembly: The future of distributed computing

### Speaker

1. Liam Randall is the co-founder of [WasmCloud](https://wasmcloud.com/), a distributed applications framework for micro services.

### Wasm value proposition

1. <u>Efficient</u> and <u>fast</u>. Near-native execution.
2. <u>Safe</u> and <u>secure</u>. Sandboxed environment. Deny-by-default design, where you need to explicitly grant capabilities.
3. <u>Open</u> and <u>debuggable</u>.
4. <u>Polyglot</u>.
5. <u>Portable</u>. Servers, browsers, embedded systems, etc.

### Wasm in Modern Distributed Computing Environment

1. The figure below shows [the Edge continuum according to the Linux Foundation Edge](https://www.lfedge.org/wp-content/uploads/2020/07/LFedge_Whitepaper.pdf). This includes a variety of device types, architectures, and capabilities. The edge ecosystem is quite complex.

![edge_continuum](/Users/jeya/Documents/projects/learn/wasm/CNCF Cloud Wasm Day/images/edge_continuum.png)

2. Evolution of a declarative (highly decoupled) world over time. More and more of the platform (blue) is being abstracted away, leaving the developer (green) to focus only on the app logic. Wasm has expanded the <u>applicability of the technology across the distributed continuum</u>, while also improving security and portability.

![tech_evolution](/Users/jeya/Documents/projects/learn/wasm/CNCF Cloud Wasm Day/images/tech_evolution.png)

3. **Examples of Wasm technology across the distributed continuum**
   * <u>Decentralized Data Centers</u>: [wasmCloud](https://github.com/wasmCloud/wasmCloud) (execution of microservices on servers), [Krustlet](https://github.com/deislabs/krustlet) (orchestration service).
   * <u>Service Provider Edge</u>: [Open Policy Agent](https://www.openpolicyagent.org/) and [Envoy](https://github.com/envoyproxy/envoy-wasm) (for executing untrusted third party plugins; replacing Lua and JavaScript).
   * <u>User Edge</u>: Shopify and Fastly (deployment platform to run untrusted Wasm code). Thousands of Wasm tools on browsers and mobile phones (e.g., [Google Earth](https://medium.com/google-earth/google-earth-comes-to-more-browsers-thanks-to-webassembly-1877d95810d6), Microsoft Flight Simulator, etc.)
4. **Challenges for distributed computing and opportunities from Wasm**: 
   * Highly variable and incompatible <u>system architectures</u> across edge continuum. Wasm has diverse ecosystem runtimes (e.g., Wasmtime, Wamr, etc.)
   * Diversity of <u>application architecture</u>. Wasm's near-native performance and demonstrated use in complex stacks makes it ideal.  
   * <u>Distributed security</u>. Sandboxed environment and deny-by-default design (user needs to explicitly grant permissions to give capabilities) makes Wasm very safe and secure.
   * <u>Offline use</u>. Wasm's security, portability, and speed, all come into play.
   * <u>Distributed machine learning</u>. TensorFlow.js brings machine learning in browser with Wasm backend. Wasi-nn brings machine learning to Wasmtime.

<hr>
