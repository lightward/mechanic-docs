# Does my theme need to be updated for Mechanic?

## Tasks that use storefront JavaScript

Most Mechanic tasks don't need anything enabled in your theme. Tasks that run JavaScript in your online store do: they use Mechanic's **Online store JavaScript** setting in the theme editor.

If one of your enabled tasks needs this and setup is incomplete, Mechanic shows a **Mechanic in your theme** card on the homepage and the task page. Click **Enable Mechanic in your theme**, make sure **Online store JavaScript** is turned on under **App embeds**, and click **Save** in the theme editor. Then return to Mechanic to confirm setup. [See the setup guide](../core/tasks/advanced-settings/javascript.md#enable-mechanic-in-your-theme).

This is a one-time setup for your current theme and serves all your enabled tasks that use storefront JavaScript. You can leave it enabled when you disable a task; disabled tasks won't run on subsequent page loads. Enabling Mechanic doesn't require editing theme code.

When you publish a different theme, open Mechanic and check whether it asks you to enable Mechanic in that theme. Saving the setting in a draft theme doesn't complete setup for the published theme.

Some tasks also need custom theme changes, such as a button or a form. Follow those tasks' instructions, and check with whoever configured them before switching themes. Enabling Mechanic loads their JavaScript; it doesn't copy custom theme changes into a new theme.

If your tasks already use Mechanic's older JavaScript integration, follow the same theme setup steps. Mechanic handles the change in the background after confirming that you've saved activation in your published theme. You don't need to reinstall your tasks or remove the old integration yourself. [Learn about existing tasks](../core/tasks/advanced-settings/javascript.md#existing-tasks).

## Storefront forms

[Storefront forms](../app/forms.md) connect customer submissions to your tasks. Build the form in Mechanic, publish it, then add a **Mechanic form** block to a template in the theme editor. Select the form in the block's picker and save the theme. You do not need to write JavaScript or enable the **Online store JavaScript** app embed to use the form block.

Each theme has its own block placements. If you change themes, place the form in the new theme too; the saved questions and connected tasks remain in Mechanic. Use the form's **Theme** tab to see saved placements. [Follow the placement guide](../app/forms.md#add-the-form-to-your-theme).
