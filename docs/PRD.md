# PRD: crm-mini

| | |
|---|---|
| Status | Draft v1 |
| Owner | rizkym4l |
| Date | 2026-11-17 |
| Type | Learning project (MVP) |

## 1. Summary

crm-mini is a WhatsApp sales agent for a small online shop. The agent chats with buyers, answers product questions from a catalog, takes orders, and saves every customer and order in a simple CRM. Risky cases go to the shop owner. The owner uses a simple web page to see orders, customers, and chats.

This is a practice project. The shop ("Toko Testing") and its catalog are fake. The goal is to learn AI engineering by building a small agent that works end to end.

## 2. Problem

Small shops sell through WhatsApp. One person answers every chat by hand. Because of this:

1. Buyers wait for answers about price and stock, and some leave.
2. Orders and buyer details live only in chat history, so they get lost.
3. The owner forgets which orders are not paid yet.

## 3. Goals

1. The agent answers product questions (price, stock, description) **only** from the catalog.
2. The agent takes an order in chat and saves it with the correct items, quantities, and total.
3. Every buyer who chats is saved as a customer, with order history.
4. Risky cases go to the owner (handoff), and the agent stops replying in that chat until the owner releases it.
5. The owner sees and updates orders, customers, products, and chats on a simple web page.
6. The project teaches the core AI engineering skills: system prompts, tool calling, the agent loop, input validation, and evaluation.

## 4. Non-goals (not in the MVP)

- Payment gateway or automatic payment checks
- Shipping cost calculation (the owner adds it by hand)
- Instagram or other channels
- More than one shop or more than one owner account
- Login and user roles (the web page runs on the owner's own computer)
- Stock reservation or inventory history
- Marketing broadcasts

## 5. Anti-goals (the MVP fails if any of these happen)

- The agent states a price, stock count, or product that is not in the catalog.
- The agent gives a discount or changes a price because a buyer asks.
- The agent marks an order as paid.
- A buyer message makes the agent ignore its rules (prompt injection).
- The agent keeps replying after a handoff.

## 6. Users

| User | Needs |
|---|---|
| **Buyer** | Fast answers about products. A simple way to order in chat. |
| **Owner** | Orders and buyers in one place. A clear list of what needs action today. Control over risky chats. |

## 7. User stories

### Buyer (WhatsApp)

1. As a buyer, I ask "kaos hitam ukuran L masih ada?" and get the price and stock.
2. As a buyer, I order "2 kaos hitam L" and the agent confirms the items and total before saving.
3. As a buyer, I get payment instructions after the order is saved.
4. As a buyer, when I complain or ask for a discount, a human takes over.

### Owner (web page)

5. As the owner, I see today's new orders, unpaid orders, and open handoffs on one page.
6. As the owner, I change an order status (for example, from unpaid to paid).
7. As the owner, I see each customer and their order history.
8. As the owner, I add and edit products, prices, and stock.
9. As the owner, I read every chat and release a chat back to the agent after a handoff.
10. As the owner, I test the agent in a chat simulator on the web page, without WhatsApp.

## 8. Functional requirements

### 8.1 Agent

| ID | Requirement |
|---|---|
| A1 | The agent replies in casual, polite Indonesian. |
| A2 | The agent gets product data only through the catalog search. It never states product facts from its own knowledge. |
| A3 | Before it saves an order, the agent shows the items, quantities, unit prices, and total, and the buyer confirms. |
| A4 | The agent asks for the buyer's name and shipping address before it saves an order. |
| A5 | The system calculates order prices and totals from the catalog, not from the agent's text. |
| A6 | After an order is saved, the agent sends fixed payment instructions (bank account from settings). |
| A7 | The agent hands off to the owner when: the buyer asks for a discount or negotiates, complains, says they paid, asks something the catalog cannot answer, or asks for a human. |
| A8 | After a handoff, the agent does not reply in that chat until the owner releases it. |
| A9 | The agent remembers the conversation with each buyer (chat history per buyer). |

### 8.2 CRM data

| ID | Requirement |
|---|---|
| C1 | The system creates a customer from the WhatsApp number on the first message. This is done by code, not by the agent. |
| C2 | The customer name and address are saved when the buyer gives them during an order. |
| C3 | Order statuses: `pending_payment` → `paid` → `shipped` → `done`, or `cancelled`. |
| C4 | Only the owner can set an order to `paid`, `shipped`, `done`, or `cancelled`. |
| C5 | Every buyer and agent message is saved. |

### 8.3 Owner web page

| ID | Requirement |
|---|---|
| W1 | Dashboard: new orders today, unpaid orders, open handoffs. |
| W2 | Orders: list, filter by status, change status. |
| W3 | Customers: list, detail with orders and chat. |
| W4 | Products: list, add, edit (name, price, stock, description, active). |
| W5 | Chats: list of conversations, read messages, release handoff. |
| W6 | Chat simulator: chat with the agent as a fake buyer, using the same agent as WhatsApp. |
| W7 | Settings: shop name and payment instructions. |
| W8 | The page is simple and works on a laptop browser. Good looks are not a goal. |

### 8.4 WhatsApp

| ID | Requirement |
|---|---|
| WA1 | The system connects to one WhatsApp number by QR scan. |
| WA2 | Only private chats are answered. Group chats are ignored. |
| WA3 | The owner gets a WhatsApp message on a separate owner number when a handoff starts or a new order is saved. |

## 9. Example conversation

```
Buyer : kak, kaos hitam size L ada?
Agent : Ada kak 🙌 Kaos Polos Hitam L, harga Rp75.000, stok 12.
Buyer : mau 2 ya
Agent : Siap kak. Boleh minta nama dan alamat kirimnya?
Buyer : Sari, Jl. Melati 5 Bandung
Agent : Pesanan kakak:
        - 2x Kaos Polos Hitam L @ Rp75.000
        Total: Rp150.000 (belum termasuk ongkir)
        Nama: Sari, alamat: Jl. Melati 5 Bandung
        Sudah benar kak?
Buyer : iya bener
Agent : Pesanan tersimpan ✅ Silakan transfer ke BCA 123456 a.n. Toko Testing.
        Ongkir akan dikonfirmasi admin ya kak.
Buyer : bisa diskon ga kak?
Agent : Untuk diskon aku sambungkan ke admin dulu ya kak 🙏
        (handoff: the agent stops replying in this chat)
```

## 10. Success criteria

| Measure | Target |
|---|---|
| Test scenarios passed (see `TASKS.md`, phase 6) | ≥ 90% |
| Anti-goal tests passed (wrong price, discount, prompt injection, paid status) | 100% |
| Order totals that match the catalog | 100% |
| Agent reply time on the local model | < 15 seconds |
| The owner can do every user story 5–10 on the web page | Yes |

## 11. Risks

| Risk | Mitigation |
|---|---|
| WhatsApp blocks the number (unofficial connection) | Use a spare number. Test only with a few friends. |
| The local model calls the wrong tool or makes up data | Strict tool rules in code, temperature 0, test scenarios. |
| Long chats overflow the model's context window | Send only recent messages. Set the Ollama context length. |
| Buyer tricks the agent | Prices, totals, and statuses are set by code. The agent cannot change them. |

## 12. Open questions

None blocking. Ideas for after the MVP: Instagram, shipping cost, stock reservation, follow-up reminders, searching chat history (RAG).
