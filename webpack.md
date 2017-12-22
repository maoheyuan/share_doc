����Webpack������ƪ�͹���


�Ķ�����֮ǰ���ȿ��������webpack�������ļ������ÿһ���㶼�����Ǳ����ܴ�������ջ�Ҳ��ͱȽ����ޣ�����Կ��������ֱ��������������ʮ��ǰ����һ�����Ժܶ�ѡ��������ɻ��ǻ�һ��ʱ�������Ķ����ģ�����ɻ�һ��һ��һ��������ʧ���������ǰû��ô�Ӵ���Webpack�����������webpack����Ȥ����ô���ָ��ű������Ǹ��ᴩʼ�յ�����дһ�Σ�д���Ժ���ᷢ�����������װ׵��߽���Webpack�Ĵ��š�
// һ��������`webpack`�����ļ�
const webpack = require('webpack');<br>
const HtmlWebpackPlugin = require('html-webpack-plugin');<br>
const ExtractTextPlugin = require('extract-text-webpack-plugin');<br>

module.exports = {<br>
        entry: __dirname + "/app/main.js", //�Ѷ���ἰ��Ψһ����ļ�<br>
        output: {<br>
            path: __dirname + "/build",<br>
            filename: "bundle-[hash].js"<br>
        },<br>
        devtool: 'none',<br>
        devServer: {<br>
            contentBase: "./public", //���ط����������ص�ҳ�����ڵ�Ŀ¼<br>
            historyApiFallback: true, //����ת<br>
            inline: true,<br>
            hot: true<br>
        },<br>
        module: {<br>
            rules: [{<br>
                    test: /(\.jsx|\.js)$/,<br>
                    use: {<br>
                        loader: "babel-loader"<br>
                    },<br>
                    exclude: /node_modules/<br>
                }, {<br>
                    test: /\.css$/,<br>
                    use: ExtractTextPlugin.extract({<br>
                        fallback: "style-loader",<br>
                        use: [{<br>
                            loader: "css-loader",<br>
                            options: {<br>
                                modules: true<br>
                            }<br>
                        }, {<br>
                            loader: "postcss-loader"<br>
                        }],<br>
                    })<br>
                }<br>
            }<br>
        ]<br>
    },<br>
    plugins: [<br>
        new webpack.BannerPlugin('��Ȩ���У�����ؾ�'),<br>
        new HtmlWebpackPlugin({<br>
            template: __dirname + "/app/index.tmpl.html" //new һ����������ʵ������������صĲ���<br>
        }),<br>
        new webpack.optimize.OccurrenceOrderPlugin(),<br>
        new webpack.optimize.UglifyJsPlugin(),<br>
        new ExtractTextPlugin("style.css")<br>

    ],<br>
};<br>

ʲô��WebPack��ΪʲôҪʹ������

ΪʲҪʹ��WebPack

�ֽ�ĺܶ���ҳ��ʵ���Կ����ǹ��ܷḻ��Ӧ�ã�����ӵ���Ÿ��ӵ�JavaScript�����һ�����������Ϊ�˼򻯿����ĸ��Ӷȣ�ǰ������ӿ�ֳ��˺ܶ�õ�ʵ������

ģ�黯�������ǿ��԰Ѹ��ӵĳ���ϸ��ΪС���ļ�;
������TypeScript������JavaScript��������չ�Ŀ������ԣ�ʹ�����ܹ�ʵ��Ŀǰ�汾��JavaScript����ֱ��ʹ�õ����ԣ�����֮����ת��ΪJavaScript�ļ�ʹ���������ʶ��
Scss��less��CSSԤ������
...
��Щ�Ľ�ȷʵ������������ǵĿ���Ч�ʣ������������ǿ������ļ�������Ҫ���ж���Ĵ�������������ʶ��,���ֶ��������Ƿǳ������ģ����ΪWebPack��Ĺ��ߵĳ����ṩ������

ʲô��Webpack

WebPack���Կ�����ģ�������������������ǣ����������Ŀ�ṹ���ҵ�JavaScriptģ���Լ�������һЩ���������ֱ�����е���չ���ԣ�Scss��TypeScript�ȣ���������ת���ʹ��Ϊ���ʵĸ�ʽ�������ʹ�á�

WebPack��Grunt�Լ�Gulp�����ʲô����

��ʵWebpack������������û��̫��Ŀɱ��ԣ�Gulp/Grunt��һ���ܹ��Ż�ǰ�˵Ŀ������̵Ĺ��ߣ���WebPack��һ��ģ�黯�Ľ������������Webpack���ŵ�ʹ��Webpack�ںܶೡ���¿������Gulp/Grunt��Ĺ��ߡ�

Grunt��Gulp�Ĺ�����ʽ�ǣ���һ�������ļ��У�ָ����ĳЩ�ļ��������Ʊ��룬��ϣ�ѹ��������ľ��岽�裬����֮������Զ����������Щ����


Grunt��Gulp�Ĺ�������
Webpack�Ĺ�����ʽ�ǣ��������Ŀ����һ�����壬ͨ��һ�����������ļ����磺index.js����Webpack��������ļ���ʼ�ҵ������Ŀ�����������ļ���ʹ��loaders�������ǣ������Ϊһ�����������������ʶ���JavaScript�ļ���


Webpack������ʽ
���ʵ��Ҫ�Ѷ��߽��бȽϣ�Webpack�Ĵ����ٶȸ����ֱ�ӣ��ܴ�����಻ͬ���͵��ļ���

��ʼʹ��Webpack

�����˽���Webpack������ʽ������һ�����Ŀ�ʼѧϰʹ��Webpack��

��װ

Webpack����ʹ��npm��װ���½�һ���յ���ϰ�ļ��У��˴�����Ϊwebpack sample project�������ն���ת�����ļ��к�ִ������ָ��Ϳ�����ɰ�װ��

//ȫ�ְ�װ
npm install -g webpack
//��װ�������ĿĿ¼
npm install --save-dev webpack
��ʽʹ��Webpackǰ��׼��

��������ϰ�ļ����д���һ��package.json�ļ�������һ����׼��npm˵���ļ��������̺��˷ḻ����Ϣ��������ǰ��Ŀ������ģ�飬�Զ���Ľű�����ȵȡ����ն���ʹ��npm init��������Զ��������package.json�ļ�
npm init
�������������ն˻�����һϵ��������Ŀ���ƣ���Ŀ���������ߵ���Ϣ���������õ��ģ�����㲻׼����npm�з������ģ�飬��Щ����Ĵ𰸶�����Ҫ���س�Ĭ�ϼ��ɡ�

package.json�ļ��Ѿ������������ڱ���Ŀ�а�װWebpack��Ϊ������
// ��װWebpack
npm install --save-dev webpack
�ص�֮ǰ�Ŀ��ļ��У��������洴�������ļ���,app�ļ��к�public�ļ��У�app�ļ����������ԭʼ���ݺ����ǽ�д��JavaScriptģ�飬public�ļ����������֮���������ȡ���ļ�������ʹ��webpack������ɵ�js�ļ��Լ�һ��index.html�ļ����������������ٴ��������ļ�:
index.html --����public�ļ�����;
Greeter.js-- ����app�ļ�����;
main.js-- ����app�ļ�����;
��ʱ��Ŀ�ṹ����ͼ��ʾ


��Ŀ�ṹ
������index.html�ļ���д���������html���룬��������Ŀ���������������js�ļ������������Ȱ�֮�������js�ļ�����Ϊbundle.js��֮�����ǻ�����ϸ��������

<!-- index.html --><br>
<!DOCTYPE html><br>
<html lang="en"><br>
  <head><br>
    <meta charset="utf-8"><br>
    <title>Webpack Sample Project</title><br>
  </head><br>
  <body><br>
    <div id='root'><br>
    </div><br>
    <script src="bundle.js"></script><br>
  </body><br>
</html><br>
������Greeter.js�ж���һ�����ذ����ʺ���Ϣ��htmlԪ�صĺ���,������CommonJS�淶�����������Ϊһ��ģ�飺

// Greeter.js
module.exports = function() {<br>
  var greet = document.createElement('div');<br>
  greet.textContent = "Hi there and greetings!";<br>
  return greet;<br>
};<br>
main.js�ļ�������д���������룬���԰�Greeterģ�鷵�صĽڵ����ҳ�档

//main.js 
const greeter = require('./Greeter.js');<br>
document.querySelector("#root").appendChild(greeter());<br>
��ʽʹ��Webpack

webpack�������ն���ʹ�ã��ڻ�����ʹ�÷������£�

# {extry file}����д����ļ���·���������о�������main.js��·����
# {destination for bundled file}����д����ļ��Ĵ��·��
# ��д·����ʱ�������{}
webpack {entry file} {destination for bundled file}
ָ������ļ���webpack���Զ�ʶ����Ŀ�������������ļ���������Ҫע�����������webpack����ȫ�ְ�װ�ģ���ô�������ն���ʹ�ô�����ʱ����Ҫ����ָ������node_modules�еĵ�ַ��������������ӣ����ն���������������

# webpack��ȫ�ְ�װ�����
node_modules/.bin/webpack app/main.js public/bundle.js<br>
�������

ʹ�������д��
���Կ���webpackͬʱ������main.js ��Greeter,js,���ڴ�index.html,���Կ������½��
htmlResult1
��û�кܼ������Ѿ��ɹ���ʹ��Webpack�����һ���ļ��ˡ��������ն��н��и��ӵĲ�������ʵ�ǲ�̫���������׳���ģ�����������Webpack����һ�ָ�������ʹ�÷�����

ͨ�������ļ���ʹ��Webpack

Webpackӵ�кܶ������ıȽϸ߼��Ĺ��ܣ�����˵���ĺ������ܵ�loaders��plugins������Щ������ʵ������ͨ��������ģʽʵ�֣���������ǰ���ᵽ�ģ�������̫���������׳���ģ����õİ취�Ƕ���һ�������ļ�����������ļ���ʵҲ��һ���򵥵�JavaScriptģ�飬���ǿ��԰����е�������ص���Ϣ�������档

���������������˵�����д��������ļ����ڵ�ǰ��ϰ�ļ��еĸ�Ŀ¼���½�һ����Ϊwebpack.config.js���ļ�������������д��������ʾ�ļ����ô��룬Ŀǰ��������Ҫ�漰��������������ļ�·���ʹ�����ļ��Ĵ��·����

module.exports = {<br>
  entry:  __dirname + "/app/main.js",//�Ѷ���ἰ��Ψһ����ļ�<br>
  output: {<br>
    path: __dirname + "/public",//�������ļ���ŵĵط�<br>
    filename: "bundle.js"//���������ļ����ļ���<br>
  }<br>
}<br>
ע����__dirname����node.js�е�һ��ȫ�ֱ�������ָ��ǰִ�нű����ڵ�Ŀ¼��
�����������֮���ٴ���ļ���ֻ�����ն�������webpack(��ȫ�ְ�װ��ʹ��node_modules/.bin/webpack)����Ϳ����ˣ�����������Զ�����webpack.config.js�ļ��е�����ѡ�ʾ�����£�

��������ļ����д��
��ѧ����һ��ʹ��Webpack�ķ��������ַ������ù��Ƿ��˵������в�������û�ио���ˬ��������ǿ�����webpack(��ȫ�ְ�װ��ʹ��node_modules/.bin/webpack)����������Բ��ã����ָо��᲻���ˬ~�����������ġ�

����ݵ�ִ�д������

��������������������Ҫ����������node_modules/.bin/webpack������·����ʵ�ǱȽϷ��˵ģ�����ֵ�����ҵ���npm������������ִ�У���npm�������ú��������������ʹ�ü򵥵�npm start���������������΢�����������package.json�ж�scripts�������������ü��ɣ����÷������¡�

{
  "name": "webpack-sample-project",<br>
  "version": "1.0.0",<br>
  "description": "Sample webpack project",<br>
  "scripts": {<br>
    "start": "webpack" // �޸ĵ������JSON�ļ���֧��ע�ͣ�����ʱ�����<br>
  },<br>
  "author": "zhang",<br>
  "license": "ISC",<br>
  "devDependencies": {<br>
    "webpack": "^1.12.9"<br>
  }<br>
}<br>
ע��package.json�е�script�ᰲװһ��˳��Ѱ�������Ӧλ�ã����ص�node_modules/.bin·���������Ѱ���嵥�У�����������ȫ�ֻ��Ǿֲ���װ��Webpack���㶼����Ҫдǰ����ָ����ϸ��·���ˡ�
npm��start������һ������Ľű����ƣ��������Ա����ڣ�����������ʹ��npm start�Ϳ���ִ������ڵ���������Ӧ�Ĵ˽ű����Ʋ���start����Ҫ��������������ʱ����Ҫ������npm run {script name}��npm run build��������������������npm start���ԣ����������£�

ʹ��npm start �������
ʹ��npm start �������
����ֻ��Ҫʹ��npm start�Ϳ��Դ���ļ��ˣ���û�о���webpackҲ��������������Ҫ̫С��webpack��Ҫ��ַ�����ǿ��Ĺ���������Ҫ�޸������ļ�������ѡ�һ����������

Webpack��ǿ����

����Source Maps��ʹ���Ը����ף�

���������벻�����ԣ�����ĵ����ܼ������߿���Ч�ʣ�������ʱ��ͨ���������ļ������ǲ������ҵ������˵ĵط�����Ӧ����д�Ĵ����λ�õģ�Source Maps�����������ǽ���������ġ�

ͨ���򵥵����ã�webpack�Ϳ����ڴ��ʱΪ�������ɵ�source maps����Ϊ�����ṩ��һ�ֶ�Ӧ�����ļ���Դ�ļ��ķ�����ʹ�ñ����Ĵ���ɶ��Ը��ߣ�Ҳ�����׵��ԡ�

��webpack�������ļ�������source maps����Ҫ����devtool�������������ֲ�ͬ������ѡ�������ȱ�㣬�������£�

devtoolѡ��	���ý��
source-map	��һ���������ļ��в���һ�������ҹ�����ȫ���ļ�������ļ�������õ�source map�����������������ٶȣ�
cheap-module-source-map	��һ���������ļ�������һ��������ӳ���map��������ӳ������˴���ٶȣ�����Ҳʹ������������߹���ֻ�ܶ�Ӧ��������У����ܶ�Ӧ��������У����ţ�����Ե�����ɲ��㣻
eval-source-map	ʹ��eval���Դ�ļ�ģ�飬��ͬһ���ļ������ɸɾ���������source map�����ѡ������ڲ�Ӱ�칹���ٶȵ�ǰ��������������sourcemap�����ǶԴ���������JS�ļ���ִ�о������ܺͰ�ȫ���������ڿ����׶�����һ���ǳ��õ�ѡ��������׶���һ����Ҫ�������ѡ�
cheap-module-eval-source-map	�����ڴ���ļ�ʱ��������source map�ķ��������ɵ�Source Map ��ʹ�����JavaScript�ļ�ͬ����ʾ��û����ӳ�䣬��eval-source-mapѡ��������Ƶ�ȱ�㣻
�����ϱ�����������ѡ�����ϵ��´���ٶ�Խ��Խ�죬����ͬʱҲ����Խ��Խ��ĸ������ã��Ͽ�Ĵ���ٶȵĺ�����ǶԴ������ļ��ĵ�ִ����һ��Ӱ�졣

��С�����͵���Ŀ�У�eval-source-map��һ���ܺõ�ѡ��ٴ�ǿ����ֻӦ�ÿ����׶�ʹ���������Ǽ����������½���webpack.config.js��������������:

module.exports = {<br>
  devtool: 'eval-source-map',<br>
  entry:  __dirname + "/app/main.js",<br>
  output: {<br>
    path: __dirname + "/public",<br>
    filename: "bundle.js"<br>
  }<br>
}<br>
cheap-module-eval-source-map���������ٶȸ��죬���ǲ����ڵ��ԣ��Ƽ��ڴ�����Ŀ����ʱ��ɱ�ʱʹ�á�
ʹ��webpack�������ط�����

�벻������������������Ĵ�����޸ģ����Զ�ˢ����ʾ�޸ĺ�Ľ������ʵWebpack�ṩһ����ѡ�ı��ؿ�����������������ط���������node.js����������ʵ������Ҫ����Щ���ܣ���������һ���������������webpack�н�������֮ǰ��Ҫ������װ����Ϊ��Ŀ����

npm install --save-dev webpack-dev-server
devserver��Ϊwebpack����ѡ���е�һ�����������һЩ����ѡ��������ÿɲο�����

devserver������ѡ��	��������
contentBase	Ĭ��webpack-dev-server��Ϊ���ļ����ṩ���ط������������Ϊ����һ��Ŀ¼�µ��ļ��ṩ���ط�������Ӧ������������������Ŀ¼���������õ���public"Ŀ¼��
port	����Ĭ�ϼ����˿ڣ����ʡ�ԣ�Ĭ��Ϊ��8080��
inline	����Ϊtrue����Դ�ļ��ı�ʱ���Զ�ˢ��ҳ��
historyApiFallback	�ڿ�����ҳӦ��ʱ�ǳ����ã���������HTML5 history API���������Ϊtrue�����е���ת��ָ��index.html
����Щ����ӵ�webpack�������ļ��У����ڵ������ļ�webpack.config.js������ʾ

module.exports = {<br>
  devtool: 'eval-source-map',<br>

  entry:  __dirname + "/app/main.js",<br>
  output: {<br>
    path: __dirname + "/public",<br>
    filename: "bundle.js"<br>
  },<br>

  devServer: {<br>
    contentBase: "./public",//���ط����������ص�ҳ�����ڵ�Ŀ¼<br>
    historyApiFallback: true,//����ת<br>
    inline: true//ʵʱˢ��<br>
  } <br>
}<br>
��package.json�е�scripts�������������������Կ������ط�������

  "scripts": {<br>
    "test": "echo \"Error: no test specified\" && exit 1",<br>
    "start": "webpack",<br>
    "server": "webpack-dev-server --open"<br>
  },<br>
���ն�������npm run server�����ڱ��ص�8080�˿ڲ鿴���

�������ط�����
Loaders

����������Loaders�ǳ��ˣ�

Loaders��webpack�ṩ��������ĵĹ���֮һ�ˡ�ͨ��ʹ�ò�ͬ��loader��webpack�����������ⲿ�Ľű��򹤾ߣ�ʵ�ֶԲ�ͬ��ʽ���ļ��Ĵ�������˵����ת��scssΪcss�����߰���һ����JS�ļ���ES6��ES7)ת��Ϊ�ִ���������ݵ�JS�ļ�����React�Ŀ������ԣ����ʵ�Loaders���԰�React�����õ���JSX�ļ�ת��ΪJS�ļ���

Loaders��Ҫ������װ������Ҫ��webpack.config.js�е�modules�ؼ����½������ã�Loaders�����ð������¼����棺

test��һ������ƥ��loaders�������ļ�����չ����������ʽ�����룩
loader��loader�����ƣ����룩
include/exclude:�ֶ���ӱ��봦����ļ����ļ��У������β���Ҫ������ļ����ļ��У�����ѡ����
query��Ϊloaders�ṩ���������ѡ���ѡ��
����������loader֮ǰ�����ǰ�Greeter.js����ʺ���Ϣ����һ��������JSON�ļ���,��ͨ�����ʵ�����ʹGreeter.js���Զ�ȡ��JSON�ļ���ֵ�����ļ��޸ĺ�Ĵ������£�

��app�ļ����д��������ʺ���Ϣ��JSON�ļ�(����Ϊconfig.json)

{
  "greetText": "Hi there and greetings from JSON!"
}
���º��Greeter.js

var config = require('./config.json');<br>

module.exports = function() {<br>
  var greet = document.createElement('div');<br>
  greet.textContent = config.greetText;<br>
  return greet;<br>
};<br>
ע ����webpack3.*/webpack2.*�Ѿ����ÿɴ���JSON�ļ��������������������webpack1.*��Ҫ��json-loader���ڿ���ξ���ʹ��loader֮ǰ�����ȿ���Babel��ʲô��
Babel

Babel��ʵ��һ������JavaScript��ƽ̨������ǿ��֮�������ڿ���ͨ���������ﵽ����Ŀ�ģ�

ʹ����һ����JavaScript���루ES6��ES7...������ʹ��Щ��׼Ŀǰ��δ����ǰ���������ȫ��֧�֣�
ʹ�û���JavaScript��������չ�����ԣ�����React��JSX��
Babel�İ�װ������

Babel��ʵ�Ǽ���ģ�黯�İ�������Ĺ���λ�ڳ�Ϊbabel-core��npm���У�webpack���԰��䲻ͬ�İ�������һ��ʹ�ã�����ÿһ������Ҫ�Ĺ��ܻ���չ���㶼��Ҫ��װ�����İ����õ������ǽ���Es6��babel-preset-es2015���ͽ���JSX��babel-preset-react������

��������һ���԰�װ��Щ������

// npmһ���԰�װ�������ģ�飬ģ��֮���ÿո����
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
��webpack������Babel�ķ�������:

module.exports = {<br>
    entry: __dirname + "/app/main.js",//�Ѷ���ἰ��Ψһ����ļ�<br>
    output: {<br>
        path: __dirname + "/public",//�������ļ���ŵĵط�<br>
        filename: "bundle.js"//���������ļ����ļ���<br>
    },<br>
    devtool: 'eval-source-map',<br>
    devServer: {<br>
        contentBase: "./public",//���ط����������ص�ҳ�����ڵ�Ŀ¼<br>
        historyApiFallback: true,//����ת<br>
        inline: true//ʵʱˢ��<br>
    },<br>
    module: {<br>
        rules: [<br>
            {<br>
                test: /(\.jsx|\.js)$/,<br>
                use: {<br>
                    loader: "babel-loader",<br>
                    options: {<br>
                        presets: [<br>
                            "es2015", "react"<br>
                        ]<br>
                    }<br>
                },<br>
                exclude: /node_modules/<br>
            }<br>
        ]<br>
    }
};
�������webpack�������Ѿ�������ʹ��ES6�Լ�JSX���﷨�ˡ���������������ӽ��в��ԣ�����������ǻ�ʹ��React���ǵ��Ȱ�װ React �� React-DOM

npm install --save react react-dom
����������ʹ��ES6���﷨������Greeter.js������һ��React���

//Greeter,js
import React, {Component} from 'react'
import config from './config.json';

class Greeter extends Component{
  render() {
    return (
      <div>
        {config.greetText}
      </div>
    );
  }
}

export default Greeter
�޸�main.js���£�ʹ��ES6��ģ�鶨�����ȾGreeterģ��

// main.js
import React from 'react';
import {render} from 'react-dom';
import Greeter from './Greeter';

render(<Greeter />, document.getElementById('root'));
����ʹ��npm start��������֮ǰ�򿪵ı��ط�����û�йرգ���Ӧ�ÿ�����localhost:8080�¿�����֮ǰһ�������ݣ���˵��react��es6����������ˡ�

localhost:8080
localhost:8080
Babel������

Babel��ʵ������ȫ�� webpack.config.js �н������ã����ǿ��ǵ�babel���зǳ��������ѡ��ڵ�һ��webpack.config.js�ļ��н�����������ʹ������ļ��Ե�̫���ӣ����һЩ������֧�ְ�babel������ѡ�����һ����������Ϊ ".babelrc" �������ļ��С��������ڵ�babel�����ò����㸴�ӣ�����֮�����ǻ��ټ�һЩ����������������Ǿ���ȡ����ز��֣������������ļ��������ã�webpack���Զ�����.babelrc���babel����ѡ������£�

module.exports = {
    entry: __dirname + "/app/main.js",//�Ѷ���ἰ��Ψһ����ļ�
    output: {
        path: __dirname + "/public",//�������ļ���ŵĵط�
        filename: "bundle.js"//���������ļ����ļ���
    },
    devtool: 'eval-source-map',
    devServer: {
        contentBase: "./public",//���ط����������ص�ҳ�����ڵ�Ŀ¼
        historyApiFallback: true,//����ת
        inline: true//ʵʱˢ��
    },
    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader"
                },
                exclude: /node_modules/
            }
        ]
    }
};

//.babelrc
{
  "presets": ["react", "es2015"]
}
��ĿǰΪֹ�������Ѿ�֪���ˣ�����ģ�飬Webpack���ṩ�ǳ�ǿ��Ĵ����ܣ�����Щ��ģ���ء�

һ�н�ģ��

Webpack��һ�����ɲ�˵���ŵ㣬�������е��ļ���������ģ�鴦��JavaScript���룬CSS��fonts�Լ�ͼƬ�ȵ�ͨ�����ʵ�loader�����Ա�����

CSS

webpack�ṩ�������ߴ�����ʽ��css-loader �� style-loader�����ߴ��������ͬ��css-loaderʹ���ܹ�ʹ������@import �� url(...)�ķ���ʵ�� require()�Ĺ���,style-loader�����еļ�������ʽ����ҳ���У����������һ��ʹ���ܹ�����ʽ��Ƕ��webpack������JS�ļ��С�

�������������

//��װ
npm install --save-dev style-loader css-loader
//ʹ��
module.exports = {

   ...
    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader"
                },
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: [
                    {
                        loader: "style-loader"
                    }, {
                        loader: "css-loader"
                    }
                ]
            }
        ]
    }
};
��ע�������ͬһ���ļ�������loader�ķ�����
����������app�ļ����ﴴ��һ������Ϊ"main.css"���ļ�����һЩԪ��������ʽ

/* main.css */
html {
  box-sizing: border-box;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}

*, *:before, *:after {
  box-sizing: inherit;
}

body {
  margin: 0;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

h1, h2, h3, h4, h5, h6, p, ul {
  margin: 0;
  padding: 0;
}
���������������õ���webpackֻ�е�һ����ڣ�������ģ����Ҫͨ�� import, require, url��������ļ������������Ϊ����webpack���ҵ���main.css���ļ������ǰ������롱main.js ���У�����

//main.js
import React from 'react';
import {render} from 'react-dom';
import Greeter from './Greeter';

import './main.css';//ʹ��require����css�ļ�

render(<Greeter />, document.getElementById('root'));
ͨ������£�css���js�����ͬһ���ļ��У���������Ϊһ��������css�ļ�������ͨ�����ʵ�����webpackҲ���԰�css���Ϊ�������ļ��ġ�
����Ĵ���˵��webpack����ô��css����ģ�鿴���ģ����Ǽ�����һ��������ʵ��cssģ��ʵ����

CSS module

�ڹ�ȥ��һЩ���JavaScriptͨ��һЩ�µ��������ԣ����õĹ����Լ����õ�ʵ������������˵ģ�黯����չ�÷ǳ�Ѹ�١�ģ��ʹ�ÿ����߰Ѹ��ӵĴ���ת��ΪС�ģ��ɾ��ģ�����������ȷ�ĵ�Ԫ������Ż����ߣ���������ͼ��ع�������Զ���ɡ�

����ǰ�˵�����һ���֣�CSS��չ�������һЩ��������ʽ��ȴ���ɾ޴��ҳ�����ȫ��������ά�����޸Ķ��ǳ����ѡ�

�����һ������ CSS modules �ļ��������ڰ�JS��ģ�黯˼�����CSS������ͨ��CSSģ�飬���е�������������Ĭ�϶�ֻ�����ڵ�ǰģ�顣Webpack��һ��ʼ�Ͷ�CSSģ�黯�ṩ��֧�֣���CSS loader�н������ú�������Ҫ����һ�о��ǰѡ�modules�����ݵ�����Ҫ�ĵط���Ȼ��Ϳ���ֱ�Ӱ�CSS���������ݵ�����Ĵ����У���������ֻ�Ե�ǰ�����Ч�����ص����ڲ�ͬ��ģ����ʹ����ͬ��������ɳ�ͻ������Ĵ�������

module.exports = {

    ...

    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader"
                },
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: [
                    {
                        loader: "style-loader"
                    }, {
                        loader: "css-loader",
                        options: {
                            modules: true
                        }
                    }
                ]
            }
        ]
    }
};
��app�ļ����´���һ��Greeter.css�ļ�

.root {
  background-color: #eee;
  padding: 10px;
  border: 3px solid #ccc;
}
����.root��Greeter.js��

import React, {Component} from 'react';
import config from './config.json';
import styles from './Greeter.css';//����

class Greeter extends Component{
  render() {
    return (
      <div className={styles.root}>//�������
        {config.greetText}
      </div>
    );
  }
}

export default Greeter
����ʹ�ðѣ���ͬ������Ҳ������ɲ�ͬ���֮�����Ⱦ��

Ӧ����css module�����ʽ
Ӧ����css module�����ʽ
CSS modules Ҳ��һ���ܴ�����⣬����Ȥ�Ļ�����ȥ�ٷ��ĵ��鿴������Ϣ

CSSԤ������

Sass �� Less ֮���Ԥ�������Ƕ�ԭ��CSS����չ������������ʹ��������variables, nesting, mixins, inheritance�Ȳ�������CSS�е�������дCSS��CSSԤ������������Щ�������͵����ת��Ϊ�������ʶ���CSS��䣬

�����ڿ��ܶ��Ѿ���Ϥ�ˣ���webpack��ʹ�����loaders�������þͿ���ʹ���ˣ������ǳ��õ�CSS ����loaders:

Less Loader
Sass Loader
Stylus Loader
������ʵҲ����һ��CSS�Ĵ���ƽ̨-PostCSS�������԰������CSSʵ�ָ���Ĺ��ܣ�����ٷ��ĵ����˽�������֪ʶ��

������˵���ʹ��PostCSS������ʹ��PostCSS��ΪCSS�����Զ������Ӧ��ͬ�������CSSǰ׺��

���Ȱ�װpostcss-loader �� autoprefixer���Զ����ǰ׺�Ĳ����

npm install --save-dev postcss-loader autoprefixer
����������webpack�����ļ������postcss-loader���ڸ�Ŀ¼�½�postcss.config.js,��������´���֮������ʹ��npm start���ʱ����д��css���Զ�����Can i use���������Ӳ�ͬǰ׺�ˡ�

//webpack.config.js<br>
module.exports = {<br>
    ...<br>
    module: {<br>
        rules: [<br>
            {<br>
                test: /(\.jsx|\.js)$/,<br>
                use: {<br>
                    loader: "babel-loader"<br>
                },<br>
                exclude: /node_modules/<br>
            },<br>
            {<br>
                test: /\.css$/,<br>
                use: [<br>
                    {<br>
                        loader: "style-loader"<br>
                    }, {<br>
                        loader: "css-loader",<br>
                        options: {<br>
                            modules: true<br>
                        }<br>
                    }, {<br>
                        loader: "postcss-loader"<br>
                    }<br>
                ]<br>
            }<br>
        ]<br>
    }<br>
}<br>
// postcss.config.js<br>
module.exports = {<br>
    plugins: [<br>
        require('autoprefixer')<br>
    ]<br>
}<br>
���ˣ������Ѿ�̸���˴���JS��Babel�ʹ���CSS��PostCSS�Ļ����÷���������ʵҲ������������ƽ̨�����webpack���Ժܺõķ������ǵ����á�����������Webpack����һ���ǳ���Ҫ�Ĺ���-Plugins

�����Plugins��

�����Plugins����������չWebpack���ܵģ����ǻ�������������������Ч��ִ����ص�����
Loaders��Plugins������Ū�죬����������ʵ����ȫ��ͬ�Ķ�����������ô��˵��loaders���ڴ��������������������Դ�ļ��ģ�JSX��Scss��Less..����һ�δ���һ�����������ֱ�Ӳ��������ļ�����ֱ�Ӷ������������������á�

Webpack�кܶ����ò����ͬʱҲ�кܶ�����������������������ɸ��ӷḻ�Ĺ��ܡ�

ʹ�ò���ķ���

Ҫʹ��ĳ�������������Ҫͨ��npm��װ����Ȼ��Ҫ���ľ�����webpack�����е�plugins�ؼ��ֲ�����Ӹò����һ��ʵ����plugins��һ�����飩������������ӣ����������һ��������������Ӱ�Ȩ�����Ĳ����

const webpack = require('webpack');<br>

module.exports = {<br>
...<br>
    module: {<br>
        rules: [<br>
            {<br>
                test: /(\.jsx|\.js)$/,<br>
                use: {<br>
                    loader: "babel-loader"<br>
                },<br>
                exclude: /node_modules/<br>
            },<br>
            {<br>
                test: /\.css$/,<br>
                use: [<br>
                    {<br>
                        loader: "style-loader"<br>
                    }, {<br>
                        loader: "css-loader",<br>
                        options: {<br>
                            modules: true<br>
                        }<br>
                    }, {<br>
                        loader: "postcss-loader"<br>
                    }<br>
                ]<br>
            }<br>
        ]<br>
    },<br>
    plugins: [<br>
        new webpack.BannerPlugin('��Ȩ���У�����ؾ�')<br>
    ],<br>
};<br>
ͨ����������������JS�ļ���ʾ����

��Ȩ���У�����ؾ�
�����webpack����Ļ����÷��ˣ����������Ƽ��������õĲ��

HtmlWebpackPlugin

������������������һ���򵥵�index.htmlģ�壬����һ���Զ������������JS�ļ�����index.html������ÿ�����ɵ�js�ļ����Ʋ�ͬʱ�ǳ����ã����������hashֵ����

��װ

npm install --save-dev html-webpack-plugin
�������Զ����������֮ǰ�ֶ�����һЩ���飬����ʽʹ��֮ǰ��Ҫ��һֱ��������Ŀ�ṹ��һЩ���ģ�

�Ƴ�public�ļ��У����ô˲����index.html�ļ����Զ����ɣ�����CSS�Ѿ�ͨ��ǰ��Ĳ��������JS���ˡ�
��appĿ¼�£�����һ��index.tmpl.html�ļ�ģ�壬���ģ�����title�ȱ���Ԫ�أ��ڱ�������У���������ݴ�ģ���������յ�htmlҳ�棬���Զ������������ css, js��favicon���ļ���index.tmpl.html�е�ģ��Դ�������£�
<!DOCTYPE html><br>
<html lang="en"><br>
  <head><br>
    <meta charset="utf-8"><br>
    <title>Webpack Sample Project</title><br>
  </head><br>
  <body><br>
    <div id='root'><br>
    </div>
  </body><br>
</html><br>
3.����webpack�������ļ�������ͬ��,�½�һ��build�ļ�������������յ�����ļ�<br>

const webpack = require('webpack');<br>
const HtmlWebpackPlugin = require('html-webpack-plugin');<br>

module.exports = {<br>
    entry: __dirname + "/app/main.js",//�Ѷ���ἰ��Ψһ����ļ�<br>
    output: {<br>
        path: __dirname + "/build",<br>
        filename: "bundle.js"<br>
    },<br>
    devtool: 'eval-source-map',<br>
    devServer: {<br>
        contentBase: "./public",//���ط����������ص�ҳ�����ڵ�Ŀ¼<br>
        historyApiFallback: true,//����ת<br>
        inline: true//ʵʱˢ��<br>
    },<br>
    module: {<br>
        rules: [<br>
            {<br>
                test: /(\.jsx|\.js)$/,<br>
                use: {<br>
                    loader: "babel-loader"<br>
                },<br>
                exclude: /node_modules/<br>
            },<br>
            {<br>
                test: /\.css$/,<br>
                use: [<br>
                    {<br>
                        loader: "style-loader"<br>
                    }, {<br>
                        loader: "css-loader",<br>
                        options: {<br>
                            modules: true<br>
                        }<br>
                    }, {<br>
                        loader: "postcss-loader"<br>
                    }<br>
                ]<br>
            }<br>
        ]<br>
    },<br>
    plugins: [<br>
        new webpack.BannerPlugin('��Ȩ���У�����ؾ�'),<br>
        new HtmlWebpackPlugin({<br>
            template: __dirname + "/app/index.tmpl.html"//new һ����������ʵ������������صĲ���<br>
        })<br>
    ],<br>
};<br>
�ٴ�ִ��npm start��ᷢ�֣�build�ļ�������������bundle.js��index.html��


build�ļ���
Hot Module Replacement

Hot Module Replacement��HMR��Ҳ��webpack������õ�һ������������������޸����������Զ�ˢ��ʵʱԤ���޸ĺ��Ч����

��webpack��ʵ��HMRҲ�ܼ򵥣�ֻ��Ҫ����������

��webpack�����ļ������HMR�����
��Webpack Dev Server����ӡ�hot��������
������������Щ��JSģ����ʵ���ǲ����Զ��ȼ��صģ�����Ҫ�����JSģ����ִ��һ��Webpack�ṩ��API����ʵ���ȼ��أ���Ȼ���API����ʹ�ã����������Reactģ�飬ʹ�������Ѿ���Ϥ��Babel���Ը������ʵ�ֹ����ȼ��ء�

���������ǵ�˼·������ʵ�ַ�������

Babel��webpack�Ƕ����Ĺ���
���߿���һ����
���߶�����ͨ�������չ����
HMR��һ��webpack��������������������ʵʱ�۲�ģ���޸ĺ��Ч���������������������������Ҫ��ģ����ж������
Babel��һ������react-transform-hrm�Ĳ���������ڲ���Reactģ����ж�������õ�ǰ������HMR����������
���Ǽ���������ʵ�ʿ����������

const webpack = require('webpack');<br>
const HtmlWebpackPlugin = require('html-webpack-plugin');<br>

module.exports = {<br>
    entry: __dirname + "/app/main.js",//�Ѷ���ἰ��Ψһ����ļ�<br>
    output: {<br>
        path: __dirname + "/build",<br>
        filename: "bundle.js"<br>
    },<br>
    devtool: 'eval-source-map',<br>
    devServer: {<br>
        contentBase: "./public",//���ط����������ص�ҳ�����ڵ�Ŀ¼<br>
        historyApiFallback: true,//����ת<br>
        inline: true,<br>
        hot: true<br>
    },<br>
    module: {<br>
        rules: [<br>
            {<br>
                test: /(\.jsx|\.js)$/,<br>
                use: {<br>
                    loader: "babel-loader"<br>
                },<br>
                exclude: /node_modules/<br>
            },<br>
            {<br>
                test: /\.css$/,<br>
                use: [<br>
                    {<br>
                        loader: "style-loader"<br>
                    }, {<br>
                        loader: "css-loader",<br>
                        options: {<br>
                            modules: true<br>
                        }<br>
                    }, {<br>
                        loader: "postcss-loader"<br>
                    }<br>
                ]<br>
            }<br>
        ]<br>
    },<br>
    plugins: [<br>
        new webpack.BannerPlugin('��Ȩ���У�����ؾ�'),<br>
        new HtmlWebpackPlugin({<br>
            template: __dirname + "/app/index.tmpl.html"//new һ����������ʵ������������صĲ���<br>
        }),<br>
        new webpack.HotModuleReplacementPlugin()//�ȼ��ز��<br>
    ],<br>
};<br>
   
��װreact-transform-hmr

npm install --save-dev babel-plugin-react-transform react-transform-hmr
����Babel

// .babelrc<br>
{<br>
  "presets": ["react", "es2015"],<br>
  "env": {<br>
    "development": {<br>
    "plugins": [["react-transform", {<br>
       "transforms": [{<br>
         "transform": "react-transform-hmr",<br>
         
         "imports": ["react"],<br>
         
         "locals": ["module"]<br>
       }]<br>
     }]]<br>
    }<br>
  }<br>
}<br>
���ڵ���ʹ��Reactʱ�������ȼ���ģ����,ÿ�α��������������Ͽ����������ݡ�

��Ʒ�׶εĹ���

ĿǰΪֹ�������Ѿ�ʹ��webpack������һ�������Ŀ��������������ڲ�Ʒ�׶Σ����ܻ���Ҫ�Դ�����ļ����ж���Ĵ�������˵�Ż���ѹ���������Լ�����CSS��JS��

���ڸ��ӵ���Ŀ��˵����Ҫ���ӵ����ã���ʱ��ֽ������ļ�Ϊ���С���ļ�����ʹ�����龮���������������������˵�����Ǵ���һ��webpack.production.config.js���ļ�����������ϻ���������,����ԭʼ��webpack.config.js��������

// webpack.production.config.js<br>
const webpack = require('webpack');<br>
const HtmlWebpackPlugin = require('html-webpack-plugin');<br>

module.exports = {<br>
    entry: __dirname + "/app/main.js", //�Ѷ���ἰ��Ψһ����ļ�<br>
    output: {<br>
        path: __dirname + "/build",<br>
        filename: "bundle.js"<br>
    },<br>
    devtool: 'eval-source-map',<br>
    devServer: {<br>
        contentBase: "./public", //���ط����������ص�ҳ�����ڵ�Ŀ¼<br>
        historyApiFallback: true, //����ת<br>
        inline: true,<br>
        hot: true<br>
    },<br>
    module: {<br>
        rules: [{<br>
            test: /(\.jsx|\.js)$/,<br>
            use: {<br>
                loader: "babel-loader"<br>
            },<br>
            exclude: /node_modules/<br>
        }, {<br>
            test: /\.css$/,<br>
            use: ExtractTextPlugin.extract({<br>
                fallback: "style-loader",<br>
                use: [{<br>
                    loader: "css-loader",<br>
                    options: {<br>
                        modules: true<br>
                    }<br>
                }, {<br>
                    loader: "postcss-loader"<br>
                }],<br>
            })<br>
        }]<br>
    },<br>
    plugins: [<br>
        new webpack.BannerPlugin('��Ȩ���У�����ؾ�'),<br>
        new HtmlWebpackPlugin({<br>
            template: __dirname + "/app/index.tmpl.html" //new һ����������ʵ������������صĲ���<br>
        }),<br>
        new webpack.HotModuleReplacementPlugin() //�ȼ��ز��<br>
    ],<br>
};<br>
//package.json<br>
{<br>
  "name": "test",<br>
  "version": "1.0.0",<br>
  "description": "",<br>
  "main": "index.js",<br>
  "scripts": {<br>
    "test": "echo \"Error: no test specified\" && exit 1",<br>
    "start": "webpack",<br>
    "server": "webpack-dev-server --open",<br>
    "build": "NODE_ENV=production webpack --config ./webpack.production.config.js --progress"<br>
  },
  "author": "",<br>
  "license": "ISC",<br>
  "devDependencies": {<br>
...<br>
  },<br>
  "dependencies": {<br>
    "react": "^15.6.1",<br>
    "react-dom": "^15.6.1"<br>
  }<br>
}<br>
�Ż����<br>

webpack�ṩ��һЩ�ڷ����׶ηǳ����õ��Ż���������Ǵ��������webpack����������ͨ��npm��װ��ͨ�����²��������ɲ�Ʒ�����׶�����Ĺ���<br>

OccurenceOrderPlugin :Ϊ�������ID��ͨ��������webpack���Է��������ȿ���ʹ������ģ�飬��Ϊ���Ƿ�����С��ID<br>
UglifyJsPlugin��ѹ��JS���룻<br>
ExtractTextPlugin������CSS��JS�ļ�<br>
���Ǽ������������������������ǣ�OccurenceOrder �� UglifyJS plugins �������ò��������Ҫ����ֻ�ǰ�װ���������ò��<br>

npm install --save-dev extract-text-webpack-plugin<br>
�������ļ���plugins����������<br>

// webpack.production.config.js<br>
const webpack = require('webpack');<br>
const HtmlWebpackPlugin = require('html-webpack-plugin');<br>
const ExtractTextPlugin = require('extract-text-webpack-plugin');<br>

module.exports = {<br>
    entry: __dirname + "/app/main.js",//�Ѷ���ἰ��Ψһ����ļ�<br>
    output: {<br>
        path: __dirname + "/build",<br>
        filename: "bundle.js"<br>
    },<br>
    devtool: 'none',<br>
    devServer: {<br>
        contentBase: "./public",//���ط����������ص�ҳ�����ڵ�Ŀ¼<br>
        historyApiFallback: true,//����ת<br>
        inline: true,<br>
        hot: true<br>
    },<br>
    module: {<br>
        rules: [<br>
            {<br>
                test: /(\.jsx|\.js)$/, <br>
                use: {<br>
                    loader: "babel-loader"<br>
                },<br>
                exclude: /node_modules/<br>
            },<br>
            {<br>
                test: /\.css$/,<br>
                use: [<br>
                    {<br>
                        loader: "style-loader"<br>
                    }, {<br>
                        loader: "css-loader",<br>
                        options: {<br>
                            modules: true<br>
                        }<br>
                    }, {<br>
                        loader: "postcss-loader"<br>
                    }<br>
                ]<br>
            }<br>
        ]<br>
    },<br>
    plugins: [<br>
        new webpack.BannerPlugin('��Ȩ���У�����ؾ�'),<br>
        new HtmlWebpackPlugin({<br>
            template: __dirname + "/app/index.tmpl.html"<br>
        }),<br>
        new webpack.optimize.OccurrenceOrderPlugin(),<br>
        new webpack.optimize.UglifyJsPlugin(),<br>
        new ExtractTextPlugin("style.css")<br>
    ],<br>
};<br>
��ʱִ��npm run build���Կ��������Ǳ�ѹ�����


ѹ����Ĵ���
����

�����޴����ڣ�ʹ�û������÷����Ǳ�֤����ļ������ļ�������ƥ��ģ����ݸı䣬������Ӧ�ı䣩

webpack���԰�һ����ϣֵ��ӵ�������ļ����У�ʹ�÷�������,���������ַ�������壨[name], [id] and [hash]��������ļ���ǰ

const webpack = require('webpack');<br>
const HtmlWebpackPlugin = require('html-webpack-plugin');<br>
const ExtractTextPlugin = require('extract-text-webpack-plugin');<br>

module.exports = {<br>
..<br>
    output: {<br>
        path: __dirname + "/build", <br>
        filename: "bundle-[hash].js" <br>
    }, <br>
   ... <br>
}; <br>
�����û����к���Ļ����ˡ�


��hashֵ��js��
�ܽ�

��ʵ����һ��ǰ�������ˣ�����ĩ�������к��޸���һ�£��������еĴ��붼�����������У�����webpack�������µ�webpack3.5.3��ϣ�������ܶ����а�����
����һƪ�ó������£�лл������ģ�����ϸ�����������Ű����ǰ�ҵ�һ���Լ�һ����������Ŀ�����Webpack���һֱ��дһƪ�ʼ����ܽᣬ���ζ��ʶ��������Լ����⣬�ܾ���д�������ֱ���������ĵ�Ӣ�İ�Webpack for React������ж�λ�Ȼ���ʵĸо���ϲ����ԭ�ĵĵ����ӾͿ��Կ��ˡ���ʵ����Webpack���Ľ������Բ���ȫ�����������㿴����Ѿ�����Webpack�Ĵ��ţ��ܹ����õ�̽�������Ĺ���Webpack��֪ʶ�ˡ�

��ӭ������ĺ󷢱��Լ��Ĺ۵����ۡ�

������Դ��http://www.jianshu.com/p/42e11515c10f