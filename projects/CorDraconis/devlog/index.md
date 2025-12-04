---
layout: default
---

<div class="page-container">
  <article>
    <h2>Cor Draconis Devlog</h2>
    <p>Here are my biweekly devlogs for my preproduction work on Cor Draconis in Fall 2024. Each post includes detailed thoughts, images, and hour tracking.</p>
  </article>

  <div class="tabbed-navigation">
    {% assign logs = site.devlogs | sort: "sprint" %}
    {% for log in logs %}
      <button class="sprint-button" onclick="showSprint({{ log.sprint }})">
        Sprint {{ log.sprint }}
      </button>
    {% endfor %}
  </div>

{% for log in logs %}
<div id="sprint{{ log.sprint }}" class="sprint-content" style="display: none;">
<h2>{{ log.title }}</h2>
<p><em>{{ log.date | date: "%B %d, %Y" }}</em></p>
<p>{{ log.summary }}</p>
<a href="{{ log.url | relative_url }}" class="read-more">Read More</a>
</div>
{% endfor %}
</div>


<script>
function showSprint(n) {
    const sections = document.querySelectorAll('.sprint-content');
    const buttons = document.querySelectorAll('.sprint-button');

    sections.forEach(s => s.style.display = 'none');
    buttons.forEach(b => b.classList.remove('active'));

    const target = document.getElementById(`sprint${n}`);
    const button = buttons[n];

    if (target) target.style.display = 'block';
    if (button) button.classList.add('active');
}

// Show first sprint on page load
document.addEventListener('DOMContentLoaded', () => {
    showSprint(0);
});


</script>