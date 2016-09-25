# php批量生成静态文件

<?php 
require_once './config.inc.php';
$m = new Model();
$ids = $m->getAll("SELECT id FROM article ORDER BY id ASC");
foreach ($ids as $rowIdArr) {

    $idStr .= $rowIdArr['id'].',';
}
$idStr = rtrim($idStr, ','); // 所有文章的 ID 号集合
$idArr = explode(',', $idStr); // 分割成数组
// 下面的程序循环生成静态页面
foreach ($idArr as $articleId) {
    $re = $m->getAll("SELECT id,title,date,author,source,content FROM article WHERE id =". $articleId); // $re 为每篇文章的内容，注意：其类型为：PDOStatement
    $article = array(); // $article 为一个数组，保存每篇文章的title、date、author、content、source
    foreach ($re as $r) {
        $article = array(
                    'title'=>$r['title'],
                    'date'=>$r['date'],
                    'author'=>$r['author'],
                    'source'=>$r['source'],
                    'content'=>$r['content']
                );
    }
    $articlePath = ROOT_PATH. '/article'; // $articlePath 为静态页面放置的目录
    if (!is_dir($articlePath)) mkdir($articlePath, 0777); // 检查目录是否存在，不存在则创建
    $fileName = ROOT_PATH . '/article/' . $articleId . '.html'; // $fileName 生成的静态文件名，格式：文章ID.html（主键ID不可能冲突）
    $articleTemPath = ROOT_PATH . '/templates/article.html'; // $articleTemPath 文章模板路径
    $articleContent = file_get_contents($articleTemPath); // 获取模板里面的内容
    // 对模板里面设置的变量进行替换。即比如：把模板里面的 <{title}> 替换成数据库里读取的 title，替换完毕赋值给变量 $articleContent
    $articleContent = getArticle(array_keys($article), $articleContent, $article);
    $resource = fopen($fileName, 'w');
    file_put_contents($fileName, $articleContent); // 写入 HTML 文件
}

/**
*  getArticle($arr, $content, $article) 对模板进行替换操作
*  @param array $arr 替换变量数组
*  @param string $content 模板内容
*  @param array $article 每篇文章内容数组，格式：array('title'=>xx, 'date'=>xx, 'author'=>xx, 'source'=>xx, 'content'=>xx);
    */
   function getArticle($arr, $content, $article) {
    // 循环替换
    foreach ($arr as $item) {
        $content = str_replace('<{'. $item .'}>', $article[$item], $content);
    }
    return $content;
   }
   ?>