<!--
EXAMPLE:
```svelte
	<script>
		import Md from 'svelte-md';
	</script>

	<Md>
		# Hello world!
		This is my MarkDown code. Isn't it cool!
	</Md>
```

CAVEAT:
* Svelte control statements do not create text which is parsable.
* For example, an 'each' loop creates a seperate childNode for each word, and does not retain whitespace information.
* (A childNode is seperated by HTML tags or Svelte control statements)
-->

<script>
  import marked from 'marked'

  export let indentLevel = 'from-second-line'
	let source;
	let render;

	function textToHtml(text) {
		if (text.includes('\t')) {
			console.warn('Markdown text contains tab character, please use spaces for consistency.')
		}
		let lines = text.replaceAll('\t', '  ').split('\n');
		// Get the expected indentation level
		if (indentLevel === 'from-second-line') {
			indentLevel = lines.length >= 2 ? lines[1].length - lines[1].trimStart().length : 0;
		}
		// Trim whitespace up to indentation level, or until the first character appears
		lines = lines.map(e => (e.length - e.trimStart().length < indentLevel) ? e.trimStart() : e.substring(indentLevel));
		return marked(lines.join('\n'));
	}

	function renderMd(){
		// Set HTML of the renderer
		render.innerHTML = "";
		source.childNodes.forEach(child => {
			if (child.nodeName === '#text') {
				render.insertAdjacentHTML('beforeend', textToHtml(child.textContent))
			} else {
				render.appendChild(child)
			}
		});
	}

	$: {
		if(source) renderMd();
	}
</script>

<div style="display: none;" bind:this={source}><slot></slot></div>
<div bind:this={render}></div>
