<!DOCTYPE html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta http-equiv="X-UA-Compatible" content="chrome=1">
    <link rel="stylesheet" href="{{ '/assets/css/style.css?v=' | append: site.github.build_revision | relative_url }}">
    <script src="https://code.jquery.com/jquery-3.2.1.min.js"
			integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
			crossorigin="anonymous"></script>
    <script src="/assets/js/moment.min.js"></script>
    <script src="/assets/js/jquery-3.2.1.min.js"></script>
    <title>Philadelphia Perl Mongers</title>
  </head>

  <body>

    <div class="container">
      <section id="main_content">
        {{ content }}
      </section>
    </div>

  </body>
  <script>
  var url = 'https://phl.matatu.org/events/next.json';

  $(document).ready( function() {
    $.ajax({
        dataType:'json',
        method:'get',
        url:url,
        success:function(r) {
            var events = r.results;
            var next_event = events.shift();
            var when = moment(next_event.time);
            $('ul#next_event').append('<li>'
                + when.format('MMMM Do, h:mm a')
                + ' at ' + next_event.venue.name
                + ' (' + when.fromNow() + ')'
                + '<br>'
                + '<a target="_blank" href="https://phl.matatu.org/event/' + next_event.id  + '">'
                + next_event.name
                + '</a>'
            );
            $(events.slice(0,3)).each(function(i,e) {
                d = moment(e.time);
                $('ul#future_events').append('<li>' + d.format("dddd, MMMM Do, h:mm a") + '</li>');
            })
        },
        error:function(jx,status,error) {
            console.log('error connecting to events api',error)
        }
        })
    })
  </script>
</html>
