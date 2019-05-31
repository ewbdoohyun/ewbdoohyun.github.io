---
layout: post
comments: true
title:  "helmet을 통한 보안 강화"
category: [backend, nodejs]
---

Node를 사용하면서 Helmet을 통해 보안을 손쉽게 강화 할 수 있다.

```
yarn add helmet 
```

(yarn을 더 좋아한다. (npm 보다는.))

​	

```
class App {
    public app: GraphQLServer;
    public pubSub: any;
    constructor(){
        this.pubSub = new PubSub;
        this.pubSub.ee.setMaxListeners(99);
        this.app = new GraphQLServer({
            schema,
            context: req => {
                const {connection: {context = null} = {} } = req;
                return {
                    req: req.request,
                    pubSub: this.pubSub,
                    context
                }
            }
        });
        this.middlewares();
    }
    private middlewares = () : void => {
        this.app.express.use(cors());
        this.app.express.use(logger("dev"));
        this.app.express.use(helmet());
    };

}
```

다음과 같이 사용하면 되고, default로 제공 되는 것 은 다음과 같다.

default option

- [dnsPrefetchControl](https://helmetjs.github.io/docs/dns-prefetch-control) controls browser DNS prefetching
- [frameguard](https://helmetjs.github.io/docs/frameguard/) to prevent clickjacking
- [hidePoweredBy](https://helmetjs.github.io/docs/hide-powered-by) to remove the X-Powered-By header
- [hsts](https://helmetjs.github.io/docs/hsts/) for HTTP Strict Transport Security
- [ieNoOpen](https://helmetjs.github.io/docs/ienoopen) sets X-Download-Options for IE8+
- [noSniff](https://helmetjs.github.io/docs/dont-sniff-mimetype) to keep clients from sniffing the MIME type
- [xssFilter](https://helmetjs.github.io/docs/xss-filter) adds some small XSS protections

외에도 다양한 option들이 존재하며, 기본적으로 다음과 같은 것들을 지원한다.

하나씩 뜯어 보자면

​	dnsPrefetchControl : 브라우저에서 사용자가 링크를 클릭 or 리소스 로드 전 요청하기 전 사전에 DNS 요청을 시작할 수 있다. 이는 클릭 했을 때 성능을 향상시키나, 방문하지 않은 것을 방문하는 것 처럼 여길 수 있다.

​	frameguard : 원하지 않는 것을 클릭하는 것을 막는다.

​	hidePoweredBy : X-Powered-by를 통해 서버에 어떤 기술이 적용 되는지 나타낼 수 있으며, 이를 숨겨준다. 덤으로 원할 경우 다른 것에 의해 구동 되는 것 처럼 보이게 할 수도 있다.

​	hsts : Https 고정

​	ieNoOpen : 악성 HTML 다운로드가 Site Context에서 실행되는 것을 방지 할 수 있다.
​	The `X-Download-Options` header can be set to `noopen`. This will prevent old versions of Internet Explorer from allowing malicious HTML downloads to be executed in the context of your site.

​	noSniff : 확장자 변환 공격을 방지 ( .jpg라는 파일을 열라고 수행하지만 해당 내용이 실제 html이라거나..)

​	xssFilter : Xss 공격 방지	

How it works 탭에 각각에 대한 설명이 더욱 더 자세하게 나와 있으니 참고 하였으면 한다.
(한가할 때 한 가지당 한 글로 정리해서 포스팅 해 보아야 겠다..)



출처 : https://www.npmjs.com/package/helmet