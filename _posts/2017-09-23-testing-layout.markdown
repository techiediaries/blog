---
layout: post
title: "RxJS Advanced Tutorial With Angular & Web Speech: Part 1"
description: "Build an app to learn about the power and flexibility of RxJS in Angular while exploring speech recognition with Web Speech API."
date: 2017-09-20 8:30
category: Technical guide, Angular, Angular 4
banner:
  text: "Auth0 makes it easy to add authentication to your Angular application."
author:
  name: "Kim Maida"
  url: "https://twitter.com/KimMaida"
  mail: "kim.maida@auth0.com"
  avatar: "https://en.gravatar.com/userimage/20807150/4c9e5bd34750ec1dcedd71cb40b4a9ba.png"
design:
  image: https://cdn.auth0.com/blog/madlibs/Logo.png
  bg_color: "#222228"
tags:
  - rxjs
  - angular
  - javascript
  - web api
related:
  - 2017-09-21-serverless-framework-and-auth0-webtasks-hop-on-the-bullet-train
  - 2017-09-20-rxjs-advanced-tutorial-with-angular-web-speech-part-1
  - 2017-09-19-building-an-app-with-Nette-and-adding-authentication
---

## This is the first header

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec ornare urna ut elit aliquet convallis. In hac habitasse platea dictumst. Mauris volutpat eros et nisi aliquet vehicula. Vivamus in neque hendrerit, tristique lorem non, pellentesque diam. Nam ut auctor odio. Mauris mollis, nibh scelerisque dignissim accumsan, justo felis vulputate est, nec placerat magna libero nec elit. Cras luctus ac massa interdum commodo. Morbi ultrices aliquam tellus, eget tempor nisl. In hac habitasse platea dictumst.

Mauris in congue augue. Duis placerat finibus laoreet. Interdum et malesuada fames ac ante ipsum primis in faucibus. Fusce non purus mauris. Maecenas vitae condimentum ligula. Mauris tempor orci libero, in sagittis purus maximus eu. Quisque porta scelerisque dictum. Vivamus eu commodo felis, nec faucibus elit. Suspendisse potenti. Donec in efficitur sapien, ut laoreet lacus. Nulla tristique varius sem ac facilisis.

## Second header Test

Maecenas hendrerit varius nunc in ultricies. Aliquam eget lacus mollis, maximus lectus lobortis, commodo risus. Quisque dignissim nulla sit amet egestas sollicitudin. Nunc vel malesuada tellus. Sed gravida enim quam. Maecenas pretium quis sem vel luctus. Donec euismod augue at justo volutpat dignissim a a massa. Phasellus sagittis consequat nibh in ultricies. Fusce dignissim ultricies elementum. Nulla congue leo luctus urna condimentum elementum. Curabitur sem nisl, fermentum at interdum ac, sollicitudin a massa. Nulla faucibus odio sed felis scelerisque ornare nec fringilla arcu. Cras gravida ligula id magna cursus, eget gravida ante laoreet.

### Subheader Test

Sed laoreet ullamcorper tempor. Nulla facilisi. Aliquam ac augue tempus, egestas ante non, pretium eros. Aliquam convallis, sapien et pellentesque ornare, ligula libero porta metus, in tincidunt tellus velit sed odio. Integer nec arcu vel justo luctus bibendum eu nec mauris. Sed feugiat nibh vitae leo commodo auctor. Duis sem mauris, sagittis ut convallis in, efficitur id ex. Praesent et dapibus nulla, vitae finibus nisl. Donec at ultrices mi. Quisque at orci sit amet nisi pellentesque facilisis eget eu nisi. Maecenas lacinia laoreet luctus. Nam eu metus sem. In eleifend pulvinar magna, non tristique elit laoreet eu. Integer finibus mauris eu placerat placerat.

Sed quis neque nec libero ornare accumsan facilisis sit amet lectus. Nullam id justo ut mauris ornare accumsan. In hac habitasse platea dictumst. Nullam et elit eu eros euismod pretium. Fusce interdum blandit ligula eu fringilla. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Integer rhoncus felis vel pretium finibus. Nunc dapibus luctus mi eget convallis. Phasellus semper faucibus massa a accumsan. Etiam feugiat viverra quam, non laoreet dolor tristique vel. Fusce tincidunt tellus risus, ac consequat lectus sagittis quis. Integer nec libero ut mauris laoreet aliquet. Fusce sit amet semper purus.

```java
// only comment can be here
package world;

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

#### Subsubheader Test

Sed laoreet ullamcorper tempor. Nulla facilisi. Aliquam ac augue tempus, egestas ante non, pretium eros. Aliquam convallis, sapien et pellentesque ornare, ligula libero porta metus, in tincidunt tellus velit sed odio. Integer nec arcu vel justo luctus bibendum eu nec mauris. Sed feugiat nibh vitae leo commodo auctor. Duis sem mauris, sagittis ut convallis in, efficitur id ex. Praesent et dapibus nulla, vitae finibus nisl. Donec at ultrices mi. Quisque at orci sit amet nisi pellentesque facilisis eget eu nisi. Maecenas lacinia laoreet luctus. Nam eu metus sem. In eleifend pulvinar magna, non tristique elit laoreet eu. Integer finibus mauris eu placerat placerat.

1. Lorem ipsum dolor sit amet
2. Donec ornare urna ut elit
3. Sed laoreet ullamcorper tempor
4. Nulla facilisi
5. Aliquam ac augue tempus

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec ornare urna ut elit aliquet convallis. In hac habitasse platea dictumst. Mauris volutpat eros et nisi aliquet vehicula. Vivamus in neque hendrerit, tristique lorem non, pellentesque diam. Nam ut auctor odio. Mauris mollis, nibh scelerisque dignissim accumsan, justo felis vulputate est, nec placerat magna libero nec elit. Cras luctus ac massa interdum commodo. Morbi ultrices aliquam tellus, eget tempor nisl. In hac habitasse platea dictumst.

Mauris in congue augue. Duis placerat finibus laoreet. Interdum et malesuada fames ac ante ipsum primis in faucibus. Fusce non purus mauris. Maecenas vitae condimentum ligula. Mauris tempor orci libero, in sagittis purus maximus eu. Quisque porta scelerisque dictum. Vivamus eu commodo felis, nec faucibus elit. Suspendisse potenti. Donec in efficitur sapien, ut laoreet lacus. Nulla tristique varius sem ac facilisis.

#### Subsubheader Test

Sed laoreet ullamcorper tempor. Nulla facilisi. Aliquam ac augue tempus, egestas ante non, pretium eros. Aliquam convallis, sapien et pellentesque ornare, ligula libero porta metus, in tincidunt tellus velit sed odio. Integer nec arcu vel justo luctus bibendum eu nec mauris. Sed feugiat nibh vitae leo commodo auctor. Duis sem mauris, sagittis ut convallis in, efficitur id ex. Praesent et dapibus nulla, vitae finibus nisl. Donec at ultrices mi. Quisque at orci sit amet nisi pellentesque facilisis eget eu nisi. Maecenas lacinia laoreet luctus. Nam eu metus sem. In eleifend pulvinar magna, non tristique elit laoreet eu. Integer finibus mauris eu placerat placerat.

Sed laoreet ullamcorper tempor. Nulla facilisi. Aliquam ac augue tempus, egestas ante non, pretium eros. Aliquam convallis, sapien et pellentesque ornare, ligula libero porta metus, in tincidunt tellus velit sed odio. Integer nec arcu vel justo luctus bibendum eu nec mauris. Sed feugiat nibh vitae leo commodo auctor. Duis sem mauris, sagittis ut convallis in, efficitur id ex. Praesent et dapibus nulla, vitae finibus nisl. Donec at ultrices mi. Quisque at orci sit amet nisi pellentesque facilisis eget eu nisi. Maecenas lacinia laoreet luctus. Nam eu metus sem. In eleifend pulvinar magna, non tristique elit laoreet eu. Integer finibus mauris eu placerat placerat.

{% include tweet_quote.html quote_text="An observable is a stream that represents a collection over time." %}

Sed quis neque nec libero ornare accumsan facilisis sit amet lectus. Nullam id justo ut mauris ornare accumsan. In hac habitasse platea dictumst. Nullam et elit eu eros euismod pretium. Fusce interdum blandit ligula eu fringilla. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Integer rhoncus felis vel pretium finibus. Nunc dapibus luctus mi eget convallis. Phasellus semper faucibus massa a accumsan. Etiam feugiat viverra quam, non laoreet dolor tristique vel. Fusce tincidunt tellus risus, ac consequat lectus sagittis quis. Integer nec libero ut mauris laoreet aliquet. Fusce sit amet semper purus.

> Sed laoreet ullamcorper tempor. Nulla facilisi. Aliquam ac augue tempus, egestas ante non, pretium eros. Aliquam convallis, sapien et pellentesque ornare, ligula libero porta metus, in tincidunt tellus velit sed odio. Integer nec arcu vel justo luctus bibendum eu nec mauris. Sed feugiat nibh vitae leo commodo auctor. Duis sem mauris, sagittis ut convallis in, efficitur id ex

### Subheader Test

Sed laoreet ullamcorper tempor. Nulla facilisi. Aliquam ac augue tempus, egestas ante non, pretium eros. Aliquam convallis, sapien et pellentesque ornare, ligula libero porta metus, in tincidunt tellus velit sed odio. Integer nec arcu vel justo luctus bibendum eu nec mauris. Sed feugiat nibh vitae leo commodo auctor. Duis sem mauris, sagittis ut convallis in, efficitur id ex

- Sed laoreet ullamcorper tempor.
- Nulla facilisi.
- Aliquam ac augue tempus.
- Gestas ante non

### Subheader Test

Sed laoreet ullamcorper tempor. Nulla facilisi. Aliquam ac augue tempus, egestas ante non, pretium eros. Aliquam convallis, sapien et pellentesque ornare, ligula libero porta metus, in tincidunt tellus velit sed odio. Integer nec arcu vel justo luctus bibendum eu nec mauris. Sed feugiat nibh vitae leo commodo auctor. Duis sem mauris, sagittis ut convallis in, efficitur id ex

{% highlight html %}
{% raw %}
<!-- src/app/listen/listen.component.html -->
<div class="alert alert-info mt-3">
  <h2 class="text-center">Speak to Play</h2>

  <p>Your browser <a class="alert-link" href="https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition#Browser_compatibility">supports speech recognition</a>! To play a madlib game using speech, follow these instructions:</p>

  <ol>
    <li>Click the <em>"Listen"</em> button below.</li>
    <li>If prompted, grant the app permission to use your device's microphone.</li>
    <li>
      Clearly say a <em>type</em> of word (also known as a "part of speech") followed by <em>one</em> word to fill in the form below. Here are some examples:
      <ul>
        <li><em>"noun <strong>cat</strong>"</em> (person, place, or thing)</li>
        <li><em>"verb <strong>jumping</strong>"</em> (action, present tense), <em>"verb <strong>ran</strong>"</em> (action, past tense)</li>
        <li><em>"adjective <strong>flashy</strong>"</em> (describing word)</li>
      </ul>
    </li>
    <li>Say <em>one command at a time</em>, then wait for the app to assess your speech to fill a madlib field. This could take a few seconds.</li>
    <li>Repeat until all fields are filled in.</li>
  </ol>

  <p>You may also <em>"Stop"</em> listening at any time and enter (or edit) words manually.</p>
</div>
{% endraw %}
{% endhighlight %}

## This is the first header

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec ornare urna ut elit aliquet convallis. In hac habitasse platea dictumst. Mauris volutpat eros et nisi aliquet vehicula. Vivamus in neque hendrerit, tristique lorem non, pellentesque diam. Nam ut auctor odio. Mauris mollis, nibh scelerisque dignissim accumsan, justo felis vulputate est, nec placerat magna libero nec elit. Cras luctus ac massa interdum commodo. Morbi ultrices aliquam tellus, eget tempor nisl. In hac habitasse platea dictumst.

Mauris in congue augue. Duis placerat finibus laoreet. Interdum et malesuada fames ac ante ipsum primis in faucibus. Fusce non purus mauris. Maecenas vitae condimentum ligula. Mauris tempor orci libero, in sagittis purus maximus eu. Quisque porta scelerisque dictum. Vivamus eu commodo felis, nec faucibus elit. Suspendisse potenti. Donec in efficitur sapien, ut laoreet lacus. Nulla tristique varius sem ac facilisis.