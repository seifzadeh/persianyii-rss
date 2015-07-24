create rss for yii2 framework
=============================
create rss uses by array or db query data

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist persianyii/yii2-rss "dev-master"
```

or add

```
"persianyii/yii2-rss": "dev-master"
```

to the require section of your `composer.json` file.


Usage
-----

Once the extension is installed, simply use it in your code by  :

```php
		$rss = new \persianyii\rss\Rss();

		$items = [];
		$items['atomLinkHref'] = '';
		$items['title'] = 'My News';
		$items['link'] = 'http://mysite.com/news.php';
		$items['description'] = 'The latest news about web-development.';
		$items['language'] = 'en-us';
		$items['generator'] = 'PHP RSS Feed Generator';
		$items['managingEditor'] = 'editor@mysite.com (Alex Jefferson)';
		$items['webMaster'] = 'webmaster@mysite.com (Vagharshak Tozalakyan)';

		$posts = (new \yii\db\Query())->
		select(['title', 'content', 'create_time'])->
		from('tbl_post')->
		where(['status' => '1'])->
		limit(20)->
		all();

		$items['items'] = [];

		foreach ($posts as $k => $v) {
			$items['items'][] = [
				'title' => $v['title'],
				'description' => substr($v['content'], 0, 500),
				'link' => 'http://yii.com/post/show/' . $v['title'] . '.html',
				'pubDate' => date('Y/m/d H:i', $v['create_time']),
			];
		}
		header('Content-Type: text/xml');
		echo $rss->createFeed($items);```