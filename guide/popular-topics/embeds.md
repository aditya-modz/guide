# Embeds

If you have been around on Discord for a bit, chances are you have seen these special messages, often sent by bots.
They can have a colored border, embedded images, text fields, and other fancy properties.

In the following section, we will explain how to compose an embed, send it, and what you need to be aware of while doing so.

## Embed preview

Here is an example of how an embed may look. We will go over embed construction in the next part of this guide.

<DiscordMessages>
	<DiscordMessage profile="bot">
		<template #embeds>
			<DiscordEmbed
				border-color="#0099ff"
				embed-title="Some title"
				url="https://discord.js.org/"
				thumbnail="https://i.imgur.com/AfFp7pu.png"
				image="https://i.imgur.com/AfFp7pu.png"
				footer-icon="https://i.imgur.com/AfFp7pu.png"
				timestamp="01/01/2018"
				author-name="Some name"
				author-icon="https://i.imgur.com/AfFp7pu.png"
				author-url="https://discord.js.org/"
			>
				Some description here
				<template #fields>
					<DiscordEmbedFields>
						<DiscordEmbedField field-title="Regular field title">
							Some value here
						</DiscordEmbedField>
						<DiscordEmbedField field-title="​">
							​
						</DiscordEmbedField>
						<DiscordEmbedField :inline="true" field-title="Inline field title">
							Some value here
						</DiscordEmbedField>
						<DiscordEmbedField :inline="true" field-title="Inline field title">
							Some value here
						</DiscordEmbedField>
						<DiscordEmbedField :inline="true" field-title="Inline field title">
							Some value here
						</DiscordEmbedField>
					</DiscordEmbedFields>
				</template>
				<template #footer>
					<span>Some footer text here</span>
				</template>
			</DiscordEmbed>
		</template>
	</DiscordMessage>
</DiscordMessages>

## Using the embed constructor

discord.js features the <DocsLink path="class/MessageEmbed" /> utility class for easy construction and manipulation of embeds.

```js
// at the top of your file
const { MessageEmbed } = require('discord.js');

// inside a command, event listener, etc.
const exampleEmbed = new MessageEmbed()
	.setColor('#0099ff')
	.setTitle('Some title')
	.setURL('https://discord.js.org/')
	.setAuthor('Some name', 'https://i.imgur.com/AfFp7pu.png', 'https://discord.js.org')
	.setDescription('Some description here')
	.setThumbnail('https://i.imgur.com/AfFp7pu.png')
	.addFields(
		{ name: 'Regular field title', value: 'Some value here' },
		{ name: '\u200B', value: '\u200B' },
		{ name: 'Inline field title', value: 'Some value here', inline: true },
		{ name: 'Inline field title', value: 'Some value here', inline: true },
	)
	.addField('Inline field title', 'Some value here', true)
	.setImage('https://i.imgur.com/AfFp7pu.png')
	.setTimestamp()
	.setFooter('Some footer text here', 'https://i.imgur.com/AfFp7pu.png');

channel.send({ embeds: [exampleEmbed] });
```

::: tip
You don't need to include all the elements showcased above. If you want a simpler embed, leave some out.
:::

The `.setColor()` method accepts a <DocsLink path="typedef/ColorResolvable" />, e.g. an integer, HEX color string, an array of RGB values or specific color strings.

To add a blank field to the embed, you can use `.addField('\u200b', '\u200b')`.

The above example chains the manipulating methods to the newly created MessageEmbed object.
If you want to modify the embed based on conditions, you will need to reference it as the constant `exampleEmbed` (for our example).

<!-- eslint-skip -->

```js
const exampleEmbed = new MessageEmbed().setTitle('Some title');

if (message.author.bot) {
	exampleEmbed.setColor('#7289da');
}
```

## Using an embed object

<!-- eslint-disable camelcase -->

```js
const exampleEmbed = {
	color: 0x0099ff,
	title: 'Some title',
	url: 'https://discord.js.org',
	author: {
		name: 'Some name',
		icon_url: 'https://i.imgur.com/AfFp7pu.png',
		url: 'https://discord.js.org',
	},
	description: 'Some description here',
	thumbnail: {
		url: 'https://i.imgur.com/AfFp7pu.png',
	},
	fields: [
		{
			name: 'Regular field title',
			value: 'Some value here',
		},
		{
			name: '\u200b',
			value: '\u200b',
			inline: false,
		},
		{
			name: 'Inline field title',
			value: 'Some value here',
			inline: true,
		},
		{
			name: 'Inline field title',
			value: 'Some value here',
			inline: true,
		},
		{
			name: 'Inline field title',
			value: 'Some value here',
			inline: true,
		},
	],
	image: {
		url: 'https://i.imgur.com/AfFp7pu.png',
	},
	timestamp: new Date(),
	footer: {
		text: 'Some footer text here',
		icon_url: 'https://i.imgur.com/AfFp7pu.png',
	},
};

channel.send({ embeds: [exampleEmbed] });
```

::: tip
You don't need to include all the elements showcased above. If you want a simpler embed, leave some out.
:::

If you want to modify the embed object based on conditions, you will need to reference it directly (as `exampleEmbed` for our example). You can then (re)assign the property values as you would with any other object.

```js
const exampleEmbed = { title: 'Some title' };

if (message.author.bot) {
	exampleEmbed.color = 0x7289da;
}
```

## Attaching images

You can upload images with your embedded message and use them as source for embed fields that support image URLs by constructing a <DocsLink path="class/MessageAttachment" /> from them to send as message option alongside the embed. The attachment parameter takes a BufferResolvable or Stream including the URL to an external image.

You can then reference and use the images inside the embed itself with `attachment://fileName.extension`.

::: tip
If you plan to attach the same image repeatedly, consider hosting it online and providing the URL in the respective embed field instead. This also makes your bot respond faster since it doesn't need to upload the image with every response depending on it.
:::

### Using the MessageEmbed builder

```js
const { MessageAttachment, MessageEmbed } = require('discord.js');
// ...
const file = new MessageAttachment('../assets/discordjs.png');
const exampleEmbed = new MessageEmbed()
	.setTitle('Some title')
	.setImage('attachment://discordjs.png');

channel.send({ embeds: [exampleEmbed], files: [file] });
```

### Using an embed object

```js
const { MessageAttachment } = require('discord.js');
// ...
const file = new MessageAttachment('../assets/discordjs.png');

const exampleEmbed = {
	title: 'Some title',
	image: {
		url: 'attachment://discordjs.png',
	},
};

channel.send({ embeds: [exampleEmbed], files: [file] });
```

::: warning
If the images don't display inside the embed but outside of it, double-check your syntax to make sure it's as shown above.
:::

## Resending and editing

We will now explain how to edit embedded message content and resend a received embed.

### Resending a received embed

To forward a received embed you retrieve it from the messages embed array (`message.embeds`) and pass it to the MessageEmbed can then be edited before sending it again.

::: warning
We deliberately create a new Embed here instead of just modifying `message.embeds[0]` directly to keep the cache valid. If we were not to do this, the embed in cache on the original message would diverge from what the actual embed looks like, which can result in unexpected behavior down the line!
:::

```js
const receivedEmbed = message.embeds[0];
const exampleEmbed = new MessageEmbed(receivedEmbed).setTitle('New title');

channel.send({ embeds: [exampleEmbed] });
```

### Editing the embedded message content

To edit the content of an embed you need to pass a new MessageEmbed structure or embed object to the messages `.edit()` method.

```js
const exampleEmbed = new MessageEmbed()
	.setTitle('Some title')
	.setDescription('Description after the edit');

message.edit({ embeds: [exampleEmbed] });
```

If you want to build the new embed data on a previously sent embed template, make sure to read the caveats in the previous section. 

## Notes

- To display fields side-by-side, you need at least two consecutive fields set to `inline`
- The timestamp will automatically adjust the timezone depending on the user's device
- Mentions of any kind will only render correctly in titles, descriptions, and field values
- Mentions in embeds will not trigger a notification
- Embeds allow masked links (e.g. `[Guide](https://discordjs.guide/ 'optional hovertext')`), but only in description and field values

## Embed limits

There are a few limits to be aware of while planning your embeds due to the API's limitations. Here is a quick reference you can come back to:

- Embed titles are limited to 256 characters
- Embed descriptions are limited to 4096 characters
- There can be up to 25 fields
- A field's name is limited to 256 characters and its value to 1024 characters
- The footer text is limited to 2048 characters
- The author name is limited to 256 characters
- The sum of all characters from all embed structures in a message must not exceed 6000 characters
- Ten embeds can be sent per message

Source: [Discord API documentation](https://discord.com/developers/docs/resources/channel#embed-limits)
