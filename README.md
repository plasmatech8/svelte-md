# svelte-md

A Svelte component to write Markdown easily - directly inside the tag!

> This solution is **NOT RECOMMENDED** for reasons stated below.
>
> Using `{@html marked('# markdown')}` is a more robust solution for writing Markdown.

## Example

```svelte
	<script>
		import Md from 'svelte-md';
	</script>

	<Md>
		# Hello world!
		This is my Markdown code. Isn't it cool!

		The normal indentation level is based on the second line ('This is my MarkDown... etc').

		We can do lists:
		* a
		* b
		* c

		And **bold** or *italics* or ~~strikethrough~~ text.

		Adding indentation makes a code block (though I recommend using triple tilde).

		Backquotes do not work. This might be because Svelte does not like seeing
		the '&gt;' character in your code.
	</Md>
```

## Caveats/Problems

### Svelte control statements

Svelte control statements do not create text which is parsable.

For example:
```svelte
List:
{#each ['a', 'b', 'c] as e}
    * {e}
{/each}
End.
```
Produces childNodes `["List:", "* ", "a", "* ", "b", "* ", "c", "End."]` instead of the desired,
`["List:\n    * a\n    * b\n    * c\nEnd."]`.

An 'each' loop creates a seperate childNode for each word, and does not retain whitespace
information.

(Note: a childNode is seperated by either HTML tags or Svelte control statements)

### Backquotes are broken

Backquotes do not work.

This might be because Svelte does not like seeing the `>` character in the middle of your HTML
tags.

### Other

There are likely other problems not mentioned. Have not explored thoroughly.