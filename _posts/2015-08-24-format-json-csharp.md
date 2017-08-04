---
layout: post
title: Форматирование JSON в C#
date: 2015-08-24 00:15
tags:
- c#
- json
---

Вы можете использовать метод `FormatJson` для получения форматированного JSON используя данный класс:

``` csharp
class JsonHelper
{
	public static string FormatJson(string str, string indentString = "\t")
	{
		var indent = 0;
		var quoted = false;
		var sb = new StringBuilder();
		for (var i = 0; i < str.Length; i++)
		{
			var ch = str[i];
			switch (ch)
			{
				case '{':
				case '[':
					sb.Append(ch);
					if (!quoted)
					{
						sb.AppendLine();
						foreach (var e in Enumerable.Range(0, ++indent))
							sb.Append(indentString);
					}
					break;
				case '}':
				case ']':
					if (!quoted)
					{
						sb.AppendLine();
						foreach (var e in Enumerable.Range(0, --indent))
							sb.Append(indentString);
					}
					sb.Append(ch);
					break;
				case '"':
					sb.Append(ch);
					bool escaped = false;
					var index = i;
					while (index > 0 && str[--index] == '\\')
						escaped = !escaped;
					if (!escaped)
						quoted = !quoted;
					break;
				case ',':
					sb.Append(ch);
					if (!quoted)
					{
						sb.AppendLine();
						foreach (var e in Enumerable.Range(0, indent))
							sb.Append(indentString);
					}
					break;
				case ':':
					sb.Append(ch);
					if (!quoted)
						sb.Append(" ");
					break;
				default:
					sb.Append(ch);
					break;
			}
		}
		return sb.ToString();
	}
}
```

[gist на GitHub](https://gist.github.com/wcoder/c24050c166b139739301)

Пример использования:

``` csharp
var output = JsonHelper.FormatJson(jsonString);
```
