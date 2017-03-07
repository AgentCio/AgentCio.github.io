---
layout: default
title: AgentC
---
<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>

<p>AgentC is a company which makes <span id="target"></span> <a href="/apps/">apps</a>.</p>



<script>
(function ($) {
  // http://stackoverflow.com/questions/13325008/typewriter-effect-with-jquery/13325547#13325547
  // writes the string
  //
  // @param jQuery $target
  // @param String str
  // @param Numeric cursor
  // @param Numeric delay
  // @param Function cb
  // @return void
  function typeString($target, str, cursor, delay, cb) {
    $target.html(function (_, html) {
      return html + str[cursor];
    });

    if (cursor < str.length - 1) {
      setTimeout(function () {
        typeString($target, str, cursor + 1, delay, cb);
      }, delay);
    }
    else {
      cb();
    }
  }

  // clears the string
  //
  // @param jQuery $target
  // @param Numeric delay
  // @param Function cb
  // @return void
  function deleteString($target, delay, cb) {
    var length;

    $target.html(function (_, html) {
      length = html.length;
      return html.substr(0, length - 1);
    });

    if (length > 1) {
      setTimeout(function () {
        deleteString($target, delay, cb);
      }, delay);
    }
    else {
      cb();
    }
  }

  // jQuery hook
  $.fn.extend({
    teletype: function (opts) {
      var settings = $.extend({}, $.teletype.defaults, opts);

      return $(this).each(function () {
        (function loop($tar, idx) {
          // type
          typeString($tar, settings.text[idx], 0, settings.delay, function () {
            // delete
            setTimeout(function () {
              deleteString($tar, settings.delay, function () {
                loop($tar, (idx + 1) % settings.text.length);
              });
            }, settings.pause);
          });

        }($(this), 0));
      });
    }
  });

  // plugin defaults
  $.extend({
    teletype: {
      defaults: {
        delay: 100,
        pause: 2500,
        text: []
      }
    }
  });
}(jQuery));

$('#target').teletype({
  text: [
    'epic',
    'titanic',
    'monumental',
    'sublime',
    'superb',
    'lovely',
    'spledniferous',
    'great',
    'swell',
    'wonderful',
    'marvelous',
    'fantastic',
    'terrific',
    'bang-up',
    'first-rate',
    'dazzling',
    'astounding'
  ]
});

$('#cursor').teletype({
  text: ['_', ' '],
  delay: 0,
  pause: 500
});
</script>