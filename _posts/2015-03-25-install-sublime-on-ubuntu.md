---
layout: post
title: Installing Sublime Text on Ubuntu14.04
---

<div class="message">
  This article includes steps to install Sublime Text on Ubuntu
</div>
<h6>Run the following commands on your terminal</h6>
* sudo add-apt-repository -y ppa:webupd8team/sublime-text-3.

<h6> If your machine is behind a proxy, we need to preserve existing environment variables using '-E' </h6>
* sudo -E add-apt-repository -y ppa:webupd8team/sublime-text-3.

<h6>Update and Install sublime:</h6>
* sudo apt-get update; sudo apt-get install -y sublime-text-installer

<p>
<div id="disqus_thread"></div>
<script>
    /**
     *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
     *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
     */
    /*
    var disqus_config = function () {
        this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        
        s.src = '//jayanthsagargithubio.disqus.com/embed.js';
        
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
</p>

