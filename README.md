# SpamTagger

This repository is currently a placeholder for future development of SpamTagger.

## What will SpamTagger be?

SpamTagger will be hard fork of MailCleaner®, by it's former head developer, which will seek to provide a very slimmed-down, easy to maintain alternative which is appropriate for use by individual nerds, SMBs with a nerd on staff, or integration into more complex environments.

While SpamTagger is in development, maintainence will be continued for MailCleaner® Community Edition. It is uncleare at the moment whether this will be possible within the existing [MailCleaner](https://github.com/MailCleaner/MailCleaner8) repository. In case this ends up not being possible a fork within this organization called [SpamTagger Plus](https://github.com/SpamTagger/SpamTagger-Plus) will be used as a direct continuation.

Until SpamTagger has an independent legacy, it will be best understood by contrast to it's predecessor. Specifically, it aims to maintain all of the filtering features of MailCleaner®, while dropping much of the fat.

| Feature                               | SpamTagger | MailCleaner® |
| :---                                  | :---:      | :---:        |
| Exim MTA                              | ✅         | ✅           |
| MailScanner Filtering Engine          | ✅         | ✅           |
| SpamAssassin Heuristic Filters        | ✅         | ✅           |
| Clam Anti-Virus                       | ✅         | ✅           |
| NiceBayes Statistical Analysis Filter | ✅         | ✅           |
| Spam Cache Filter                     | ✅         | ✅           |
| Web UI for configuration              | ⛔         | ✅           |
| Per-User Quarantines                  | ⛔         | ✅           |
| MariaDB Configuration Database        | ⛔         | ✅           |
| Advanced TUI Wizard                   | ✅         | ⛔           |
| TOML Configuration Files              | ✅         | ⛔           |
| Comprehensive Configuration API       | ✅         | ⛔           |
| Automatic Service Restart             | ✅         | ⛔           |
| Tesseract OCR                         | ✅         | ⛔           |

There will be several significant difference between MailCleaner® and SpamTagger:

* **SpamTagger will not have user quarantines.** This prominant features of MailCleaner® allows users to access the list of messages which are suspected to be a Spam/Newsletter from the appliances web interface rather than having them delivered to their mailbox. Instead, SpamTagger will provide a more robust version of MailCleaner®'s "Tag mode". With this, SpamTagger will provide a varitey of mechanisms for flagging a message as spam before ultimately delivering everything to a back-end mail server. These mechanisms will allow suspected spam messages to be delivered to the original recipient, but be filtered by mail server or mail reader rules into a Junk folder, or to be delivered to a different destination mailbox for review instead.

* **SpamTagger will store all configuration in TOML configuration files.** TOML is a very human-readable markup language which is commonly used for configuring system services. TOML is robust enough to store any relevant data still needed by SpamTagger which MailCleaner® would store in it's database.

* Given that it is able to store all configuration settings in TOML files, and it does not need to store quarantine details **SpamTagger will drop using a database entirely.**

* The above changes also make having a web interface unnecessary. So, **SpamTagger will be configurable by modifying the TOML files directly or using a text user interface (TUI), in lieu of a web interface**. This TUI will validate inputs, write the configuration files on your behalf and automatically restart necessary services.

* **SpamTagger will have a daemon which watches all configuration files for changes and will automatically restart relevant services when necessary.** It will revert changes if services fail to start after changes are made.

* **Configuration changes will be automatically version-controlled.** When using the TUI, each change will be committed to a distinct Git configuration repository with generic comments to indicate what was changed. If you manually make changes to these files, you will need to commit those changes yourself so that you can make your own comments. Warnings will be provided if your configuration repository has uncommited changes. You can then back-up and sync this configuration to different machines.

* **SpamTagger will aim to be more modern and flexible to deploy.** While this may take a while to come along, the goal will be to have a build system which can make reproducable VM images, installable ISO images, and Docker container images. Furthermore, it should be possible to provide images for different architectures, such as for the Raspberry Pi and other SBCs.

* **SpamTagger will have a comprehensive configuration API.** This will seek to allow all settings to be readable and writable externally so that other projects can seek to add other user interfaces back to SpamTagger if they wish. Specifically, it will be desirable to allow for plugins which can make SpamTagger configurable in tools like NextCloud, Plesk, CPanel and other similar hosting and domain management interfaces.

* Since SpamTagger will be the project that is being actively maintained, **it will continue to see investment in new features, focusing on filtering quality over all else.** To begin with, TesseractOCR integration should be provided from the beginning.

* **SpamTagger will seek to be almost entirely written in Perl**. While this may not be the most popular language, it has a long legacy with mail filtering and continues to be the primary language used for SpamAssassin, MailScanner, and MailCleaner®'s back-end. After removing the PHP web interface from MailCleaner®, the majority of the remaining code is Perl with some pieces of Bash and Python. Reducing as much as possible to a single language written in a standard style which is as welcoming as possible to unfamiliar developers should improve the ease of future development.
  
### Why are you targetting these changes?

Each of the features that was dropped represents a disproportionate amount of development and maintainance time needed to make MailCleaner® a robust product, while not being essential to the function of an anti-spam gateway.

Web programming languages and web frameworks frequently change and since web interfaces make for relatively huge code bases, it can be quite a burden just to keep them running. Similarly, any time a single button of field needs to be added, it can require touching a dozen files and hours of troubleshooting to make sure that it functions correctly. The various levels of the web stack also present the largest quantity of potential vulnerabilities and are generally the most easily accessible attack surface.

User quarantines, which are implemented via a web interface, have all the baggage just described as well as additional concerns. Users need to be seemlessly authenticated, need to do anything they want within a couple of clicks, and need to be able to access their quarantine from their new phone, on coffee shop Wi-Fi, without needing to remember their password, lest there be complaints and support tickets. 99% convenience is not good enough. By just delivering all of the mail, this becomes the mail server and mail readers' problem. Microsoft, Google, Mozilla, and some of the world other most highly-vetted projects are better suited to do this.

With no more need for a WebUI and no more tracking of quarantined content, there is then very little need for a proper database. Having a database is much less of a concern when it comes to development effort, security, or support because all of this is provided by the database vendor through standard interfaces. However, it is still one more service, dependend upon by all others, which can fail or become the target of an attack. Databases can be great for getting a lot of information quickly, but tend not to provide this information in the most human-friendly format and requires at least a baseline knowledge of SQL to access. By storing settings directly in configuration files they can be easily viewed and modified with a regular text editor, they can be source controlled and they can be watched by the service daemon to provide extra frills that cannot be easily implemented with a DB.

### Why do SpamTagger and SpamTagger Plus even exist?

Future development of MailCleaner® has been discontinued. SpamTagger Plus will seek to extend the life of MailCleaner® appliances for users who want as much stability as possible. However, given the complexity of the additional features discussed above, it is unrealistic to continue developing it beyond basic maintainance without substantially more resources.

The goal of SpamTagger is to have an alternative which can not only be maintained, but also see continual development with many fewer resources. It is intended to be simple and lightweight enough that work can be sufficiently done by one person on a part-time basis.

### Is SpamTagger affiliated with MailCleaner®/Alinto?

No.

SpamTagger is a project which is being lead by the former head of development for MailCleaner® without any direction from MailClenaer®/Alinto. It inherits much of it's code from MailCleaner® by virtue of MailCleaner® being an open source project. SpamTagger Plus will seek to continue to be directly compatible with upstream MailCleaner® if it continues to see any development, but SpamTagger will not be compatible except with the possibility to make a one-time, irreversable migration.

Please DO NOT contact MailCleaner®/Alinto with questions or concerns related to SpamTagger.

### Should I migrate from MailCleaner®?

SpamTagger does not yet exist in any usable form, so you cannot migrate to it. If it ends up being necessary to migrate any existing MailCleaner® appliance to the SpamTagger Plus repository, then instructions will be made available.

If you pay for Enterprise Edition then you definitely should not migrate until your contract ends since it provides some premium data feeds which cannot be made availabe for SpamTagger Plus. However, SpamTagger Plus will share all of the same features available in MailCleaner® Community Edition.

If you would like e-mail filtering provided as a cloud service and are an existing MailCleaner® Cloud customer, you should consider using Alinto's [Cleanmail](https://www.alinto.com/email-security-relay-cleanmail) product.

<!--If you don't currently use MailCleaner and would like to self-host your anti-spam gateway, you can start using [SpamTagger Plus](https://github.com/SpamTagger/SpamTagger-Plus) while SpamTagger is in development.
-->
If you would like to try a different self-hosted mail filtering solution which is even more light-weight and even more DIY than SpamTagger aims to be, you should check out [Mailmunge](https://www.mailmunge.org). This is a Milter-based tool which does not require a separate relay and which provides complete scripting freedom with Perl.

## Contributing

This project will be entirely open source and developed in the open in an effort to maximize community engagment. Contributions will be happily accepted, but please see the `CONTRIBUTING.md` document before suggesting any changes.

## See Also

[Cleanmail](https://www.alinto.com/email-security-relay-cleanmail) - The cloud-based e-mail filtering gateway from Alinto. More appropriate for hand-off, user-friendly mail filtering.

[Mailmunge](https://www.mailmunge.org) - A do-it-yourself filter written in Perl, using the Milter protocol (Sendmail and Postfix) from my friend and mentor Dianne Skoll. More appropriate for very lightweight, flexible filtering for those with development knowledge.

MailCleaner® is a registered trademark of [ALINTO](https://www.alinto.com)

SpamTagger is a copyright of John Mertz &lt;git&ZeroWidthSpace;@&ZeroWidthSpace;john.me.tz&gt; distributed under the GPLv3 license
