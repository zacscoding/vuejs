## 07. Vue.js 고급 개발자 되기

### 7.1 뷰 중고급 레벨로 올라가기 위한 지식

- #### Vuex  
; 애플리케이션의 상태 관리(State Management)를 돕는 라이브러리  
https://vuex.vuejs.org/  

- #### 뷰의 반응성(Vue Reactivity)
; 뷰가 데이터 변화를 감지했을 때 자동으로 화면을 갱신하는 특성  
(인스턴스를 생성하는 시점에 watcher 등록)  
https://vuejs.org/v2/guide/reactivity.html  

- #### 서버 사이드 렌더링  

뷰 공식 사이트 서버 사이드 렌더링 : https://ssr.vuejs.org/  
서버 사이드 렌더링 라이브러리 Nuxt.js : https://nuxtjs.org/

---  

### 7.2 뷰 개발을 위한 웹팩  
; 모듈 번들러  
=> 서로 연관이 있는 모듈 간의 관계를 해석하여 정적인 자원으로 변환해주는 변환 도구  

- #### 웹팩의 주요 속성  

<table>
  <tr>
    <td>속성</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>entry</td>
    <td>
      웹팩으로 빌드(변환)할 대상 파일을 지정하는 속성<br>
      entry로 지정한 파일의 내용에는 전체 애플리케이션 로직과 필요 라이브러리를
      로딩하는 로직이 들어감
    </td>
  </tr>
  <tr>
    <td>output</td>
    <td>
      웹팩으로 빌드한 결과물의 위치와 파일 이름 등 세부 옵션을 설정하는 속성
    </td>
  </tr>
  <tr>
    <td>loader</td>
    <td>
      웹팩으로 빌드할 때 HTML, CSS, PNG 파일 등을 자바스크립트로 변환하기 위해
      필요한 설정을 정의하는 속성
    </td>
  </tr>
  <tr>
    <td>plugin</td>
    <td>
      웹팩으로 빌드하고 나온 결과물에 대해 추가 기능을 제공하는 속성<br>
      예를들어 결과물의 사이즈를 줄이거나 기타 CSS, HTML 파일로 분리하는 기능 등      
    </td>
  </tr>
  <tr>
    <td>resolve</td>
    <td>
      웹팩으로 빌드할 때 해당 파일이 어떻게 해석되는지 정의하는 속성<br>
      예를 들어 특정 라이브러리를 로딩할 때 버전은 어떤 걸로하고, 파일 경로는
      어디로 지정하는지 등을 정의
    </td>
  </tr>  
</table>

- #### 웹팩 데브 서버(webpack-dev-server)  
; 웹팩 설정 파일의 변화를 감지하여 빠르게 웹팩을 빌드할 수 있도록 지원하는  
유틸리티이자 Node.js 서버 (인메모리)  

- #### webpack-simple 프로젝트의 웹팩 설정 파일(webpack.config.js) 분석  

> 파일 경로와 웹팩 라이브러리 로딩  

```
// output 속성에서 사용할 노드 path 라이브러리
var path = require('path')
// 웹팩 플러그인에서 사용할 라이브러리
var webpack = require('webpack')
```  

> entry 속성  

```
module.exports = {
  entry: './src/main.js',
  ...
}
```

> output 속성  

```
module.exports = {
  ...
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js'
  },
  ...
}
```  

> module 속성  
애플리케이션 파일들을 빌드할 때 HTML, CSS, PNG 등의 파일을 자바스크립트로
변환해주는 로더 지정


```
module.exports = {
  ...  
  module: {
    rules: [
      // css 속성들이 vue-style-loader를 통해 index.html에 style 태그로 삽입
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ],
      },
      // vue 파일이 js로 변환
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
          }
          // other vue-loader options go here
        }
      },
      // js 파일의 ES6 문법을 모든 브라우저에서 호환 가능한 js로 변환(transpile)
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      // 이미지 파일들은 file-loader를 이용하여 js 파일로 변환
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  }
  ...
}
```  

> resolve 속성  

웹팩으로 빌드할 때 뷰 라이브러리 선택  
vue.em.js는 최신 웹팩 버전과 사용할 수 있는 Full 버전의 라이브러리  

```
module.exports = {
  ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    },
    extensions: ['*', '.js', '.vue', '.json']
  },
  ...
}
```  

> devServer 속성  

```
module.exports = {
  ...
  devServer: {
    // 클라이언트 사이드 라우팅인 뷰 라우터와 함께 사용하기 위해 true
    historyApiFallback: true,
    // 처음 서버를 시작할 때만 웹팩 빌드 정보를 보여주고 이후 변경 시에는 빌드 정보를 보여주지 않음    
    noInfo: true,
    // 웹팩으로 빌드할 때 오류가 있으면 브라우저 화면 전체에 오류를 표시
    overlay: true
  }
  ...
}
```  

> performance 속성  

```
module.exports = {
  ...
  // 웹팩으로 빌드한 파일의 크기가 250kb를 넘으면 경고 메시지를 표시할 지 설정
  performance: {
    hints: false
  },
  ...
}
```

> devtool 속성

```
module.exports = {
  ...
  // 개발자 도구에서 사용할 디버깅 방식을 지정
  devtool: '#eval-source-map'
  ...
}
```  

---  

### 7.3 뷰 개발을 위한 ES6  

ES6란 ECMAScript 2015 or ES6라 부르고 최신 자바스크립트 문법이자 스펙  



















<br />
