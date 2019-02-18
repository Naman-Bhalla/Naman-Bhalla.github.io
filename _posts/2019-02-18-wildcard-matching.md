---
layout: post
title: "Wildcard Matching"
date: 2019-02-18 19:20:00 +0530
categories: competitive-programming
comments: true
description: Wildcard Matching
keywords: Leetcode, Wildcard Matching, Competitive Programming
---

The solution is as given at [http://yucoding.blogspot.com/2013/02/leetcode-question-123-wildcard-matching.html](http://yucoding.blogspot.com/2013/02/leetcode-question-123-wildcard-matching.html) . In the below implementation, I have tried to use variables that are self explanatory in what they are doing. I guess this shall help remove any doubts.

I had seen one doubt regarding the use of variable "match" at [https://leetcode.com/problems/wildcard-matching/discuss/17810/Linear-runtime-and-constant-space-solution](https://leetcode.com/problems/wildcard-matching/discuss/17810/Linear-runtime-and-constant-space-solution) . In my implementation, its work is done by the variable "last_non_matched_with_star" . Let me explain its use in a bit detail. When we find a "", it can either match "" or any sequence. Thus, initially, we set the variable to the index of string_pointer at the time. This is for the case when next character in the pattern shall first be compared with the present character of string. In case they don't match, the code shall go the condition "elif last_found_star != -1:"  and match the current string character with the last encountered " * " and again pattern will be pointing to the very next character.
Now, it also handles a case like:

```
pattern = a\*cabab
string = aeglcabdab
```
When \* is encountered, it matches with characters "egl" in string and then "cab" match with the pattern's corresponding characters. But, when we encounter "d" in string, we realize that "probably" sequence till "d" should have been matched with the " \* " instead and thus we start iterating from the last_non_matched_with_star again. It might be a case that infact there could have been another intermediate substring that satisfies. Thus we need the "last_non_matched_with_star" variable.

I feel it also answer doubts regarding the correct time complexity of the implementation. It is actually O(mn) where m and n are the lengths of the pattern and the string respectively. This can happen in a case when "\*" is the very first character in the pattern. It tries to match till last and later realizes that these should have been matched by the \* instead and goes back. I also tried to analyze if it is amortized O(n) but couldn't prove it. 

```
class Solution:
    def isMatch(self, s: 'str', p: 'str') -> 'bool':
        string_ptr = 0
        pattern_ptr = 0
        last_found_star = -1
        
        while string_ptr < len(s):
            
            if pattern_ptr < len(p) and (p[pattern_ptr] == "?" or s[string_ptr] == p[pattern_ptr]):
                string_ptr += 1
                pattern_ptr += 1
            elif pattern_ptr < len(p) and p[pattern_ptr] == "*":
                last_found_star = pattern_ptr
                last_non_matched_with_star = string_ptr # "*" can represent empty string too
                pattern_ptr += 1
            elif last_found_star != -1:
                pattern_ptr = last_found_star + 1
                last_non_matched_with_star += 1
                string_ptr = last_non_matched_with_star
            else:
                return False
            
        while pattern_ptr < len(p) and p[pattern_ptr] == "*":
            pattern_ptr += 1
            
        return pattern_ptr == len(p)
                
        
```

Please feel free to ask any doubts.
Thanks !

{% include  share.html %}

{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = "https://naman-bhalla.github.io/competitive-programming/2019/02/19/wildcard-matching.html";  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = "wildcard-matching"; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://namanbhalla-in.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}


<!-- Go to www.addthis.com/dashboard to customize your tools -->
<script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-597a01df538d0348"></script>
