# Chapitre 7 -- Les sous-comptes et leur financement

Un sous-compte (SubAccount) est un compte secondaire, cree et gere par l application, qui peut agir sans rouvrir la popup a chaque transaction -- utile pour des flux de jeu ou d automatisation ou une confirmation manuelle a chaque appel serait trop lourde. sdk.subAccount.create() envoie une requete wallet_addSubAccount ; Signer.addSubAccount() commence par verifier un cache local (state.subAccount) et le retourne directement si aucune adresse specifique n est demandee, evitant un aller-retour inutile vers la popup.

Pour un sous-compte de type create sans cles fournies explicitement, le Signer va chercher un compte proprietaire via subAccountsConfig.toOwnerAccount() si l application en a fourni un, sinon via getCryptoKeyAccount() -- une paire de cles WebAuthn/P-256 geree localement par le module kms/crypto-key. Cette cle proprietaire est ensuite ajoutee aux parametres de la requete avant envoi a la popup.

Le financement d un sous-compte suit par defaut le mode spend-permissions : shouldRequestUseSubAccountSigner() detecte qu une requete cible l adresse du sous-compte, et sendRequestToSubAccountSigner() verifie qu une spend permission existe deja (storeHelpers.spendPermissions) avant de signer localement via createSubAccountSigner(). Si aucune permission n existe encore pour une transaction, routeThroughGlobalAccount() est appele a la place : la transaction passe par le compte principal, qui possede l autorite complete, plutot que par le sous-compte seul. Une gestion d erreur dediee, handleInsufficientBalanceError, intercepte egalement les echecs dus a un solde insuffisant sur le sous-compte pour tenter un chemin de repli, sauf si le mode de financement est manual.

Fichiers centraux : `packages/account-sdk/src/sign/base-account/Signer.ts`, `utils/routeThroughGlobalAccount.ts`, `utils/handleInsufficientBalance.ts`.

[Chapitre suivant : Base Pay, un paiement USDC en trois lignes](08-base-pay.md)
