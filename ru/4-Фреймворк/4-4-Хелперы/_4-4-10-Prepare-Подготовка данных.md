# Подготовка данных

Пространство имен:

    use is\Helpers\Prepare;

/*
*  Класс, который производит очистку данных по заданному параметру
*  например, перед передачей ее для записи в базу данных
*  на входе нужно указать значение $data и тип преобразования $type
*  
*  параметры типов (если нужно несколько, можно перечислять через пробел и/или запятую):
*    format - оставление в строке только (!) цифр, латинских букв, пробелов, и разрешенных знаков для передачи данных в формате системы
*    alphanumeric - оставление в строке только (!) цифр, латинских букв и пробелов
*    numeric - оставление в строке только (!) цифр
*    datetime - оставление в строке только (!) цифр и знаков, встречающихся в формате даты и времени
*    phone - приведение строки к телефонному номеру
*    phone_ru - приведение строки к телефонному номеру россии (+7 заменяется на 8)
*    phone_link - приведение строки к телефонной ссылке (добавляется + вначале)
*    login/email - приведение строки к формату логина/email
*    url - приведение строки к формату url, включая спецсимволы
*    simpleurl - приведение строки к формату url без спецсимволов, с обрезкой всех параметров
*    urlencode - приведение строки к формату url, в котором символы кодируются % и hex-кодом
*    urldecode - приведение строки из формата urlencode в обычный текстовый вид
*    leavespaces - укажите, чтобы оставить по одному пробелу (если они вообще есть) в начале и в конце сторки
*    tospaces - приведение всех пробелов, табуляций и символов пробелов к одному пробелу
*    nospaces - удаление всех пробелов
*    codespaces - удаление незначащих для кода пробелов, сокращение кода
*    onestring - приведение данных к однострочному виду
*    code - htmlspecialchars
*    entities - htmlentities
*    notags - удаление всех тегов
*    cleartags - очищение всех атрибутов внутри тегов
*    tags - удаление всех тегов, кроме разрешенных
*      чтобы этот параметр работал корректно, входящие данные должны быть кодированы 
*      htmlspecialchars, в противном случае теги будут очищены
*      на предварительном этапе обработки
*  
*  теперь, если указать третий параметр 'false', то чистка тегов будет пропущена
*  т.е. все теги в тексте останутся как есть
*  если указать 'true', то будут оставлены только теги по-умолчанию
*  если же задать массив, то будут исключены все теги, кроме указанных
*  действие этого параметра не распространяется на код php и скрипты, т.к. они будут очищены в любом случае
*  
*  в функцию добавился четвертый параметр, который может быть как массивом, так и иметь значение true
*  в качестве массива он может содержать ключи 'minlen', 'maxlen', 'minnum', 'maxnum' и 'match',
*  по которым будет идти проверка входной строки, объект же будет преобразован в массив
*  также данный параметр имеет и еще одно свойство: если этот параметр не пустой
*  (например, строка или число, хотя мы настоятельно рекомендуем указывать массив или 'true'),
*  окончательная, очищенная строка будет сравниваться с исходной
*  в случае совпадения будет возвращаться очищенная строка, в противном случае - false
*  таким образом, эта функция теперь объединяет в себе очищение и проверку
*  
*  также нельзя считать эту функцию полностью безопасной, т.к. она не очистит обфусцированные и шифрованные данные
*  (т.е. переданные фрагментами), например: '<scr ipt>' или 'PHNjcmlwdD4=' (base64)
*  однако мы стараемся сделать так, чтобы все файлы системы проходили антивирусную проверку,
*  в частности через AIBolit, и не выдавать даже подозрений на вирусы,
*  так чтобы вредоносный код можно было сразу же обнаружить
*  
*  на выходе отдает преобразованное значение $data
*/

## Prepare::clear

Prepare::clear($data)

Функция 

**data**
Тип: **строка**
Строка

$data = self::script($data);
$data = self::stripTags($data);
$data = self::trim($data);
$data = self::spaces($data);

return $data;


## Prepare::script

Prepare::script($data)

Функция 

**data**
Тип: **строка**
Строка

// выполняем предварительное очищение - от скриптов, программного кода

$data = preg_replace('/<\?.+?\?>/u', '', $data);
$data = preg_replace('/<script.+?\/script>/ui', '', $data);

return $data;

## Prepare::stripTags

Prepare::stripTags($data, $tags = null)

Функция 

**data**
Тип: **строка**
Строка

**tags**
Тип: **строка**
Строка

// продолжаем предварительное очищение - от всех тегов, кроме разрешенных

// задаем разрешенные теги

if (empty($tags)) {
	$tags = [
		// base elements
		'a', 'p', 'h1', 'h2', 'h3', 'h4', 'h5', 'h6', 'pre', 'span', 'font', 'br', 'hr', 'img',
		// base formatting
		'b', 'i', 's', 'u', 'blockquote', 'code', 'del', 'em', 'ins', 'small', 'strong', 'sub', 'sup', 
		// list
		'ul', 'ol', 'li', 'dl', 'dt', 'dd', 'details', 'summary',
		// table
		'table', 'thead', 'tbody', 'tfoot', 'th', 'tr', 'td', 'col', 'colgroup', 'caption',
		// additional
		'abbr', 'bdi', 'bdo', 'cite', 'dfn', 'kbd', 'mark', 'q', 'rp', 'rt', 'rtc', 'ruby', 'samp', 'var', 'wbr'
	];
} else {
	$tags = Objects::convert($tags);
}

// подготавливаем список
$strip = '';
foreach ($tags as $tag) {
	$strip .= '<' . $tag . '>';
}

// завершаем
unset($tags, $tag);

// очищаем
$data = strip_tags($data, $strip);

return $data;

## Prepare::trim

Prepare::trim($data, $replace = null)

Функция 

**data**
Тип: **строка**
Строка

**replace**
Тип: **строка**
Строка

// убираем все пробелы и табуляцию вначале и в конце
// второй параметр задает замену при очищении
// по-умолчанию, пусто
// можно указать '$1' для замены на найденное значение

$data = preg_replace('/^(\s|(&nbsp;))+/ui', $replace, $data);
$data = preg_replace('/(\s|(&nbsp;))+$/ui', $replace, $data);

return $data;

## Prepare::spaces

Prepare::spaces($data, $replace = '$1')

Функция 

**data**
Тип: **строка**
Строка

**replace**
Тип: **строка**
Строка

// убираем двойные пробелы и табуляцию
// второй параметр задает замену при очищении
// по-умолчанию, '$1' - замена на найденное значение
// можно указать null для очищения (nospaces) или ' ' для замены на пробел (tospaces)

$data = preg_replace('/(\s|&nbsp;)+/ui', $replace, $data);

return $data;

## Prepare::comments

Prepare::comments($data)

Функция 

**data**
Тип: **строка**
Строка

// убираем комментарии

// clear comments [//...] from json parse with frotect of 'href://' string
$data = preg_replace('/([^\:\"\'])\s*?\/\/.*?([$\r\n])/u', '$1$2', $data);
// clear comments [/*...*/]
$data = preg_replace('/\/\*.*?\*\//u', '', $data);
// clear comments [<!--...-->]
$data = preg_replace('/(\<\!\-\-).*?(\-\-\>)/u', '', $data);

return $data;

## Prepare::format

Prepare::format($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/[^a-zA-Z0-9_\- .,:;]/u', '', $data);
return $data;

## Prepare::letters

Prepare::letters($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/[^\w]|\d/u', '', $data);
return $data;

## Prepare::words

Prepare::words($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/[^\w ]|\d/u', '', $data);
return $data;

## Prepare::text

Prepare::text($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/^[\w\d\s\-\'\"\.\,\!\?\(\)\:\№\*«»…—‒–]+$/u', '', $data);
return $data;

## Prepare::alphanumeric

Prepare::alphanumeric($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/[^a-zA-Z0-9_\- ]/u', '', $data);
return $data;

## Prepare::numeric

Prepare::numeric($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/^[^\d]+?(\d)/', '$1', $data);
$data = str_replace(',', '.', $data);
$data = (float) $data;
return $data;

## Prepare::datetime

Prepare::datetime($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/[^0-9_\-.,:()\\\\\/ ]/u', '', $data);
return $data;

## Prepare::phone

Prepare::phone($data, $locale = null)

Функция 

**data**
Тип: **строка**
Строка

**locale**
Тип: **строка**
Строка

$data = preg_replace('/[^0-9]/u', '', $data);
//$original = !empty($special) ? $data : null;

if ($locale === 'ru') {
	$first = mb_substr($data, 0, 1);
	if (strlen($data) == 10) {
		$data = substr_replace($data, '7', 0, 0);
		//$original = !empty($special) ? $data : null;
	} elseif ($first == 8) {
		$data = substr_replace($data, '7', 0, 1);
	}
}

return '+' . $data;

## Prepare::login

Prepare::login($data)

Функция 

**data**
Тип: **строка**
Строка

return self::email($data);

## Prepare::email

Prepare::email($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/[^a-zA-Z0-9\-_.@]/u', '', $data);
return $data;

## Prepare::url

Prepare::url($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/[^a-zA-Z0-9\-_.:\/?&\'\"=#+]/u', '', $data);
$data = rawurlencode($data);
return $data;

## Prepare::simpleurl

Prepare::simpleurl($data)

Функция 

**data**
Тип: **строка**
Строка

$data = preg_replace('/[?&].*$/u', '', $data);
$data = preg_replace('/[^a-zA-Z0-9\-_.:\/\w]/u', '', $data);
$data = htmlspecialchars($data);
return $data;

## Prepare::urlencode

Prepare::urlencode($data)

Функция 

**data**
Тип: **строка**
Строка

$data = rawurlencode($data);
return $data;

## Prepare::urldecode

Prepare::urldecode($data)

Функция 

**data**
Тип: **строка**
Строка

$data = rawurldecode($data);
//$data = preg_replace('/[^a-zA-Z0-9\-_.,:\/?&=#+\w ]/u', '', $data);
return $data;

## Prepare::onestring

Prepare::onestring($data)

Функция 

**data**
Тип: **строка**
Строка

// clear line breaks from json prepare: vvv
// $data = preg_replace('/\r\n\s*|\r\s*|\n\s*/u', '', $data);
$data = preg_replace('/([^\s]|^)[\s]*(\r?\n){1,}[\s]*([^\s]|$)/u', '$1 $3', $data);
return $data;

## Prepare::code

Prepare::code($data)

Функция 

**data**
Тип: **строка**
Строка

$data = htmlspecialchars($data, ENT_QUOTES | ENT_HTML5);
return $data;

## Prepare::entities

Prepare::entities($data)

Функция 

**data**
Тип: **строка**
Строка

$data = htmlentities($data);
return $data;

## Prepare::tags

Prepare::tags($data)

Функция 

**data**
Тип: **строка**
Строка

$data = htmlspecialchars_decode($data);
$data = strip_tags($data, $striptags);
return $data;

## Prepare::notags

Prepare::notags($data, $replace = null)

Функция 

**data**
Тип: **строка**
Строка

**replace**
Тип: **строка**
Строка

// второй параметр задает замену при очищении
// по-умолчанию, null - замена на пустое значение
// можно указать ' ' для замены на пробел (notagsspaced)

//$data = preg_replace('/([^\s\t]|^)[\s\t]*(\r?\n){1,}[\s\t]*([^\s\t]|$)/', '$1 $3', $data);
$data = preg_replace('/(<\/\w+?>)|(<\w+?\s.+?>)|(<\w+?>)/u', $replace, $data);

return $data;

## Prepare::cleartags

Prepare::cleartags($data)

Функция 

**data**
Тип: **строка**
Строка

//$data = preg_replace('/([^\s\t]|^)[\s\t]*(\r?\n){1,}[\s\t]*([^\s\t]|$)/', '$1 $3', $data);
$data = preg_replace('/<(\w+)?\s.+?>/u', '<$1>', $data);
return $data;

## Prepare::len

Prepare::len($data, $min = null, $max = null)

Функция 

**data**
Тип: **строка**
Строка

**min**
Тип: **строка**
Строка

**max**
Тип: **строка**
Строка

// раньше было specail minlen/maxlen
// сравнение original/minmun/maxnum теперь через класс match

// правило, задающее минимальную длину строки

if (
	!empty($min) &&
	is_numeric($min) &&
	$min > 0 &&
	mb_strlen($data) < $min
) {
	$data = null;
}

// правило, задающее максимальную длину строки

if (
	!empty($max) &&
	is_numeric($max) &&
	$max > 0 &&
	mb_strlen($data) > $max
) {
	$data = mb_substr($data, 0, $max);
}

return $data;

## Prepare::upper

Prepare::upper($data)

Функция 

**data**
Тип: **строка**
Строка

// правило, переводящую строку в верхний регистр

return mb_convert_case($data, MB_CASE_UPPER);

}

## Prepare::lower

Prepare::lower($data)

Функция 

**data**
Тип: **строка**
Строка

// правило, переводящую строку в верхний регистр

return mb_convert_case($data, MB_CASE_LOWER);

## Prepare::upperFirst

Prepare::upperFirst($data)

Функция 

// правило, переводящую строку в верхний регистр

return mb_convert_case($data, MB_CASE_TITLE);

}

## Prepare::upperEach

Prepare::upperEach($data)

Функция 

**data**
Тип: **строка**
Строка

// правило, переводящую строку в верхний регистр

return mb_convert_case(mb_substr($data, 0, 1), MB_CASE_UPPER) . mb_convert_case(mb_substr($data, 1), MB_CASE_LOWER);

## Prepare::encode

Prepare::encode($str)

Функция 

**str**
Тип: **строка**
Строка

Функция которая кодирует данные
на входе нужно указать исходную строку
на выходе отдает готовую строку
кодирование происходит в формат base64


if (!System::typeOf($str, 'scalar')) {
	return null;
}

$len = Strings::len($str) % 3;
if ($len) {
	$str = Strings::fillup($str, 3 - $len);
}

return base64_encode($str);

}

## Prepare::decode

Prepare::decode($str)

Функция 

**str**
Тип: **строка**
Строка

Функция которая декодирует данные
на входе нужно указать исходную строку
на выходе отдает готовую строку
декодирование происходит из формата base64


if (!System::typeOf($str, 'scalar')) {
	return null;
}

return base64_decode($str);

## Prepare::hash

Prepare::hash($str)

Функция 

**str**
Тип: **строка**
Строка

Функция которая делает хэш данных
на входе нужно указать исходную строку
на выходе отдает готовую строку


if (!System::typeOf($str, 'scalar')) {
	return null;
}

return md5($str);

## Prepare::matchHash

Prepare::matchHash($str, $hash)

Функция 

**str**
Тип: **строка**
Строка

**hash**
Тип: **строка**
Строка

Функция которая проверяет,
соответствует ли переданная строка своему хэшу
на входе нужно указать исходную строку и ее хэш
на выходе отдает результат проверки


if (!System::typeOf($str, 'scalar')) {
	return null;
}

return md5($str) === $hash;

## Prepare::crypt

Prepare::crypt($str)

Функция 

**str**
Тип: **строка**
Строка

Функция которая делает необратимое шифрование данных
на входе нужно указать исходную строку
на выходе отдает готовую строку


if (!System::typeOf($str, 'scalar')) {
	return null;
}

return password_hash($str, PASSWORD_DEFAULT);

## Prepare::matchCrypt

Prepare::matchCrypt($str, $hash)

Функция проверки хэша в необратимом шифровании

**str**
Тип: **строка**
Строка

**hash**
Тип: **строка**
Строка

Функция проверяет, соответствует ли переданная строка своему хэшу в необратимом шифровании.
на входе нужно указать исходную строку и ее хэш

Возвращает результат проверки - **true** или **false**.


if (!System::typeOf($str, 'scalar')) {
	return null;
}

return password_verify($str, $hash);
