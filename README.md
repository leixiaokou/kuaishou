###PHP解析快手短视频无水印播放地址


##### 快手无水印代码

下载Guzzle库并引入

`composer require guzzlehttp/guzzle`

```php
require __DIR__.'/vendor/autoload.php';
$url = $_GET['url'] ?? '';
$headers = [
  'Connection' => 'keep-alive',
  'User-Agent'=>'Mozilla/5.0 (iPhone; CPU iPhone OS 12_1_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16D57 Version/12.0 Safari/604.1'
];
$client = new \GuzzleHttp\Client(['timeout' => 2, 'headers' => $headers, 'http_errors' => false,]);
$data['headers'] = $headers;
$jar = new \GuzzleHttp\Cookie\CookieJar;
$data['cookies'] = $jar;
$response = $client->request('GET', $url, $data);

$body = $response->getBody();
if ($body instanceof \GuzzleHttp\Psr7\Stream) {
  $body = $body->getContents();
}
$result = htmlspecialchars_decode($body);
$pattern = '#"srcNoMark":"(.*?)"#';
preg_match($pattern, $result, $match);
$data = [
  'video_src' => '',
  'cover_image' => '',
];
if (empty($match[1])) {
  return false;
}
$data['video_src'] = $match[1];
$pattern = '#"poster":"(.*?)"#';
preg_match($pattern, $result, $match);
if (!empty($match[1])) {
  $data['cover_image'] = $match[1];
}
echo json_encode($data);
die();
```
##### 抖音 快手 微视 API集成服务
传送门 https://github.com/leixiaokou/short-video
