# reactjs-webpack

https://viblo.asia/p/huong-dan-cau-hinh-reactjs-voi-webpack-va-babel-3Q75wE19ZWb?fbclid=IwAR2XYR0900a1KDg3WTTnyp_-12NwC37HEOmfD5YS_mZD9g1HyDbzm_rvKz4

Webpack là trình đóng gói code Javascript và Babel có nhiệm vụ chuyển các đoạn code Javascript trên phiên bản mới về lại phiên bản cũ phù hợp với các trình duyệt cũ hơn. Nào chúng ta bắt đầu thôi...

1. Khởi Tạo ReactJS
   Tạo Thư Mục, File Và Khởi Tạo NPM
   Đầu tiên các bạn khởi tạo thư mục và cấu hình dự án npm mới:

$ mkdir react-webpack
$ cd react-webpack
$ npm init -y
Ở đây mình dùng npm init -y để khởi tạo nhanh dự án, bạn nào không thích có thể dùng npm init để cấu hình từng phần chi tiết nha.

Sau khi chạy xong nó sẽ tạo ra được file package.json có nội dung tương tự như sau:

{
"name": "react-webpack",
"version": "1.0.0",
"description": "",
"main": "index.js",
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1"
},
"keywords": [],
"author": "",
"license": "ISC"
}
Tiếp theo bạn tạo một file index.js và file index.html tại thư mục public ở thư mục gốc của dự án. Tạo tiếp 1 file App.js nằm ở thư mục src, đây là file mình sẽ bắt đầu viết ReactJS vào. Sau khi tạo xong thư mục của bạn sẽ tương tự thế này:

Cài Đặt ReactJS
Đầu tiên hãy cài react và react-dom với câu lệnh sau:

$ npm install --save react react-dom
Mở file index.html lên và thêm đoạn code sau:

Ở đây thẻ <noscript> dùng thể yêu cầu trình duyệt mở Javascript.

Và thẻ <div> có id root dùng để render React.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
</body>
</html>
Thêm code vào file index.js nữa nhé:

import React from "react";
import ReactDOM from "react-dom";
import App from './src/App';

ReactDOM.render(<App />, document.getElementById("root"));
Cuối cùng viết Hello World với React thôi, cũng dễ mà đúng ko 🤗, mở file App.js và thêm đoạn code sau:

import React from "react";

export default function App() {
return (
<div>
<h1>Hello World!</h1>
</div>
);
} 2. Khởi Tạo Webpack
Cài Đặt Webpack
Đầu tiên các bạn cài đặt tất cả những package sau:

$ npm install --save-dev webpack webpack-cli webpack-dev-server
webpack là gói thư viện chính.
webpack-cli dùng để chạy webpack.
webpack-dev-server dùng để chạy trong quá trình phát triển.
Cấu Hình Webpack
Tạo file webpack.config.js nằm ở thư mục gốc của dự án và thêm như sau:

const path = require("path");

module.exports = {
/_ đây là file đầu tiên mà webpack sẽ đọc ở đây mình để index.js _/
entry: path.resolve(**dirname, "index.js"),
/_ cấu hình thư mục đầu ra là dist và tên file là index.bundle.js,
clean dùng để reset thư mục dist khi build _/
output: {
path: path.resolve(**dirname, "dist"),
filename: "index.bundle.js",
clean: true,
},
};
Thêm File Bundle Vào HTML
Khi file index.bundle.js được tạo, mình cần yêu cầu webpack đưa nó làm thẻ <script> vào tệp HTML. Để làm điều đó, trước tiên chúng ta cần cài đặt một thư viện khác:

$ npm install --save-dev html-webpack-plugin
Chỉnh sửa lại file webpack.config.js như sau:

const path = require("path");
/_ thêm html-webpack-plugin vào file cấu hình _/
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
entry: path.resolve(**dirname, "index.js"),
output: {
path: path.resolve(**dirname, "dist"),
filename: "index.bundle.js",
clean: true,
},
/_ cấu hình file index.html từ folder public _/
plugins: [
new HtmlWebpackPlugin({
template: path.join(__dirname, "public", "index.html"),
}),
],
}; 3. Khởi Tạo Babel
Cài Đặt Babel
Tiếp tục cài đặt các package sau:

$ npm install --save-dev @babel/core babel-loader @babel/preset-env @babel/preset-react
@babel/core là gói thư viện của babel.
babel-loader dùng để load babel vào dự án.
@babel/preset-env dùng để chuyển code Javascript về ES5.
@babel/preset-react dùng để sử dụng babel với ReactJS.
Cấu Hình Babel
Bây giờ chúng ta cần yêu cầu webpack tải các tệp javascript bằng cách sử dụng babel trước khi đóng gói. Thêm đoạn code sau vào webpack.config.js:

const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
entry: path.resolve(**dirname, "index.js"),
output: {
path: path.resolve(**dirname, "dist"),
filename: "index.bundle.js",
clean: true,
},
/_ đoạn code sau sẽ load các gói babel vào webpack _/
module: {
rules: [
{
test: /\.js$/,
exclude: /node_modules/,
use: {
loader: "babel-loader",
options: {
presets: ["@babel/preset-env", "@babel/preset-react"],
},
},
},
],
},
plugins: [
new HtmlWebpackPlugin({
template: path.join(__dirname, "public", "index.html"),
}),
],
}; 4. Build Và Chạy Thôi
Thêm scripts vào file package.json như sau:

"scripts": {
"serve": "webpack serve --mode development",
"build": "webpack --mode production",
},
Ok bây giờ để chạy được thì bạn chỉ cần gõ npm run serve là xong, mở http://localhost:8080/ và xem thành quả nhé.

Để build dự án bạn chạy câu lệnh npm run build khi đó webpack sẽ gói lại tất cả file Javascript của bạn vào thư mục dist mà mình cấu hình trước đó.

Chạy file index.html và tận hưởng thành quả 😃.

5. Bonus
   Để sử dụng Javascript Asynchronous (trên babel 7.4 không còn dùng gói này nữa) bạn cần chỉnh lại entry như sau:
   module.exports = {
   //...
   entry: ["@babel/polyfill", path.resolve(__dirname, "index.js")],
   }
6. Kết Thúc
   Bài viết đến đây là kết thúc rồi, nếu có sai sót gì bạn bình luận phía dưới đóng góp cho mình nha. Để lại 1 vote và follow mình nếu thấy bài viết giúp ích cho bản thân ❤️.

Các nguồn tham khảo:

https://webpack.js.org/concepts/
https://viblo.asia/p/reactjs-ket-hop-voi-webpack-part-1-Eb85ogr052G
https://medium.com/age-of-awareness/setup-react-with-webpack-and-babel-5114a14a47e9
https://stackoverflow.com/questions/33527653/babel-6-regeneratorruntime-is-not-defined
