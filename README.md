# project Title
Mattiles

# Code from chat gpt
    // Import libraries
    const { Client, GatewayIntentBits } = require('discord.js');
    const client = new Client({ intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages, GatewayIntentBits.MessageContent] });

    let balances = {}; // Temporary in-memory storage for user balances

    client.once('ready', () => {
    console.log(`Logged in as ${client.user.tag}!`);
    });

    client.on('messageCreate', async (message) => {
    if (message.author.bot) return;

    // Parse command
    const [command, ...args] = message.content.split(" ");

    // !checkbalance command
    if (command === "!checkbalance") {
        const balance = balances[message.author.id] || 0;
        message.reply(`Your balance is: ${balance} coins`);
    }

    // !credit command
    else if (command === "!credit") {
        const amount = parseInt(args[0]);
        if (isNaN(amount) || amount <= 0) return message.reply("Please enter a valid amount.");
        
        balances[message.author.id] = (balances[message.author.id] || 0) + amount;
        message.reply(`You have been credited ${amount} coins. New balance: ${balances[message.author.id]}`);
    }

    // !debit command
    else if (command === "!debit") {
        const amount = parseInt(args[0]);
        if (isNaN(amount) || amount <= 0) return message.reply("Please enter a valid amount.");

        const balance = balances[message.author.id] || 0;
        if (balance < amount) return message.reply("Insufficient funds.");

        balances[message.author.id] -= amount;
        message.reply(`You have been debited ${amount} coins. New balance: ${balances[message.author.id]}`);
    }

    // !transfer command
    else if (command === "!transfer") {
        const targetUser = message.mentions.users.first();
        const amount = parseInt(args[1]);
        if (!targetUser || isNaN(amount) || amount <= 0) return message.reply("Please mention a user and specify a valid amount.");

        const senderBalance = balances[message.author.id] || 0;
        if (senderBalance < amount) return message.reply("Insufficient funds.");

        balances[message.author.id] -= amount;
        balances[targetUser.id] = (balances[targetUser.id] || 0) + amount;
        message.reply(`Transferred ${amount} coins to ${targetUser.username}. Your new balance: ${balances[message.author.id]}`);
    }

    // Add other commands for game logic here
    });

    client.login('MTMwMzUyMDEzOTc3MzIxNDc2MA.GJZptc.tJtYB6iE2E28ObCKWEwbaQYJcjvAzR2-NYT0vM');

# Clone
    git clone https://github.com/DeFiJudy/Mattiles_1.git

