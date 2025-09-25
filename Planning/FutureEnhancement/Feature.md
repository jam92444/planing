## 🔮 Future Enhancements / Roadmap

| Feature                        | Description                                                                                                                                 | Potential Impact                                                                 |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Multi-store support**        | Allow one admin to manage multiple store locations (chains/franchises). Store-level separation of inventory, billing, users, and analytics. | Scale from single stores to chains or franchises with centralized control.       |
| **Mobile version**             | Develop a mobile app or responsive UI for Android tablets or phones, enabling cashiers to bill on-the-go.                                   | Greater flexibility & faster billing on handheld devices.                        |
| **Loyalty system (Basic CRM)** | Add customer profiles with purchase history, points, rewards, and promotions.                                                               | Boost customer retention and marketing opportunities.                            |
| **Offline mode**               | Cache sales data locally when offline, sync automatically once internet is restored.                                                        | Ensure uninterrupted billing even with unstable internet connectivity.           |
| **Receipts via SMS/WhatsApp**  | Send bills/receipts digitally to customers through SMS or WhatsApp with QR codes or links.                                                  | Modern digital receipts, reduces paper usage, and enhances customer convenience. |

---

## Integration Notes

- **Multi-store support** requires extending the `store_id` logic to allow one user to belong to multiple stores with permission levels.
- **Mobile version** can be a React Native app or PWA to reuse existing React codebase.
- **Loyalty system** will need new collections like `customers`, `loyalty_points`, and APIs for managing customer data.
- **Offline mode** will require local storage, service workers, and conflict resolution strategies on sync.
- **Receipts via SMS/WhatsApp** involves integrating with SMS gateways (e.g., Twilio) or WhatsApp Business API, and generating shareable receipt links or PDFs.

---

## Milestones for Future Work

1. **Multi-store support** (high priority for scaling)
2. **Mobile app or responsive UI** (medium priority)
3. **Loyalty CRM** (medium priority for marketing)
4. **Offline mode** (advanced feature for reliability)
5. **Digital receipts via SMS/WhatsApp** (customer convenience & sustainability)
