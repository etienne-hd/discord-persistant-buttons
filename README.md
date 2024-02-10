## Discord Persistent Button Example

This example demonstrates how to create persistent buttons in Discord using the discord.py library.

### How It Works

1. **Bot Initialization:**
   - The bot is initialized with the necessary intents to handle interactions.

2. **Command Setup:**
   - The `button_test` command is defined, which creates a button with a custom ID when invoked.

3. **Button Creation:**
   - When the `button_test` command is used, it creates a button labeled "Click Me" with the provided custom ID.

4. **Interaction Handling:**
   - The `on_interaction` event handles interactions with the button.
   - It checks if a custom ID is present in the interaction data and retrieves it.

### Example Code

```python
import discord
from discord.ext import commands

# Definition of the Bot class
class Bot(commands.Bot):
    def __init__(self):
        intents = discord.Intents.all()
        super().__init__(command_prefix="?", intents=intents)

    async def setup_hook(self):
        # Synchronize data with external sources during setup
        await self.tree.sync()

    async def on_command_error(self, ctx, error):
        await ctx.reply(str(error), ephemeral=True)

# Instantiate the bot
bot = Bot()

# Define the 'button_test' command
@bot.hybrid_command(
    name="button_test",
    description="Button Test"
)
@commands.has_permissions(administrator=True)
async def button_test(
    ctx: commands.Context,
    custom_id: str,
):
    # Create a view for the button
    view = discord.ui.View()
    # Add a button to the view
    view.add_item(discord.ui.Button(label="Click Me", custom_id=custom_id))
    # Respond to the command with a message containing an embed and the button view
    await ctx.reply(embed=discord.Embed(title="Button tester", description="Click the button"), view=view)

# Define the 'on_interaction' event
@bot.event
async def on_interaction(interaction):
    # If a custom ID is present in the interaction data
    if "custom_id" in interaction.data:
        # Retrieve the custom ID of the button
        custom_id = interaction.data["custom_id"]
        print("Custom ID of the button:", custom_id)
        # INTERACTION ARG: https://discordpy.readthedocs.io/en/stable/interactions/api.html#discord.Interaction

# Launch the bot with the specified token
bot.run("Your Token")
```

### Example Usage

1. **Creating a Button:**
   - Use the `?button_test` command with a custom ID to create a button.
   ```
   ?button_test custom_id_here
   ```

2. **Interacting with the Button:**
   - After the button is created, users can interact with it by clicking.
   - The bot will print the custom ID of the clicked button.

### Customization

- You can customize the behavior of the button and its appearance by modifying the `button_test` command in the code.
- Adjust permissions and bot settings as needed for your specific use case.
