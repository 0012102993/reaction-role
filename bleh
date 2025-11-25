import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.members = True

bot = commands.Bot(command_prefix='$', intents=intents)

role_message_id = 0

emoji_to_role = {
    discord.PartialEmoji(name='🔴'): 0,
    discord.PartialEmoji(name='🟡'): 0,
    discord.PartialEmoji(name='green', id=0): 0,
}

@bot.event
async def on_raw_reaction_add(payload):
    if payload.message_id != role_message_id:
        return

    guild = bot.get_guild(payload.guild_id)
    if guild is None:
        return

    try:
        role_id = emoji_to_role[payload.emoji]
    except KeyError:
        return

    role = guild.get_role(role_id)
    if role is None:
        return

    try:
        await payload.member.add_roles(role)
    except discord.HTTPException:
        pass


@bot.event
async def on_raw_reaction_remove(payload):
    if payload.message_id != role_message_id:
        return

    guild = bot.get_guild(payload.guild_id)
    if guild is None:
        return

    try:
        role_id = emoji_to_role[payload.emoji]
    except KeyError:
        return

    role = guild.get_role(role_id)
    if role is None:
        return

    member = guild.get_member(payload.user_id)
    if member is None:
        return

    try:
        await member.remove_roles(role)
    except discord.HTTPException:
        pass


bot.run("token")
