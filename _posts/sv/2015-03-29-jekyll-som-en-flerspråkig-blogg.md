---
name: jekyll-multilingual
category: sv
---

Att sätta upp en flerspråkig blogg visade sig vara problematiskt. Jag har
experimenterat fram och tillbaka med olika system, men landade till sist på
[Jekyll] [jekyll] av ett par anledningar.

1. **Den är statisk**
	Jag gillar idén med att funktionellt generare HTML
baserat på icke HTML-innehåll

2. **Kan hostas på GitHub**
	Administrering av en fullständig linux VPS verkade lite onödigt för en
blogg, så jag blev glad av att se GitHub hosta Jekyllsidor.

3. **Den är utbygbar**
	Jekyll är väldigt lätt att bygga ut via deras objektmodell och mallramverk.

Att göra Jekyll flerspråkig
===========================

Jekyll är inte flerspråkig som standard, men det är lätt att lägga till denna
funktionalitet som jag upptäckte genom att läsa [en artikel av Sylvian Duran]
[sylvianjekml].

En sak att notera om hur Jekyll binder datastrukturerna är att efter den arbetet
igenom listan med artiklar, gör den dem tillgängliga via den
`site.posts` arrayen. Detta inkluderar alla artiklar i alla språk, så den kommer
behöva filtreras.


Att hitta nästa artikeln
========================

En funktionalitet jag ville använda som slutade fungera när jag lagt till flera
språk är `page.next` och `page.previous` variablerna som Jekyll gör tillgängliga.

Dessa variabler pekar på nästa och förgående artikel relativt till nuvarande
artikel och kommer olyckligtvis peka till en artikel av ett annat språk större
delen av tiden. Lyckligtvis så har artikeln man får ut av dessa variabler också
`next` och `previous` variabler. Vilket betyder att man kan göra
`page.next.next.next...` för att traversa arrayn i förhållande till nuvarande
artikel.

Vi kan utforska arrayn frammåt och bakåt med en rekursiv mall. Den kommer fortsätta tills den antingen stöter på en artikel av korrekt språk eller värdet `nil`.

{% highlight liquid %}
{% raw %}

{% assign next_post = include.next_post %}

{% if next_post == nil %}
	<!-- Ingen nästa artikel -->
{% elsif next_post.lang == page.lang %}
	<a href="{{ next_post.url }}" >Nästa</a>
{% else %}
	{% include next_button.html next_post=next_post.next %}
{% endif %}

{% endraw %}
{% endhighlight %}

För att använda:

{% highlight liquid %}
{% raw %}

{% include next_button.html next_post=page.next %}
{% include prev_button.html prev_post=page.previous %}

{% endraw %}
{% endhighlight %}


[jekyll]: http://jekyllrb.com
[sylvianjekml]: https://sylvain.durand.tf/making-jekyll-multilingual/
