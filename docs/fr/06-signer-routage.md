# Chapitre 6 -- Signer.request, la table de routage des methodes

Signer.request() est le coeur logique du SDK complet : une grande instruction switch sur request.method qui decide, pour chaque methode JSON-RPC ou EIP-1193, comment la traiter. Le comportement se divise d abord selon un etat binaire, this.accounts.length === 0 (aucun compte connecte) ou non.

Sans compte connecte, seule une poignee de methodes est autorisee : wallet_switchEthereumChain est traitee localement (elle ne fait que memoriser la chaine cible pour la prochaine connexion), tandis que wallet_connect, experimental_requestInfo, wallet_sendCalls et wallet_sign sont envoyees a la popup via sendRequestToPopup -- c est d ailleurs la seule facon d obtenir un compte au depart. Toute autre methode declenche une erreur provider.unauthorized invitant a appeler eth_requestAccounts en premier.

Une fois connecte, la table s elargit considerablement. Certaines methodes sont repondues localement sans aller au wallet : eth_accounts, eth_coinbase, net_version, eth_chainId lisent simplement l etat local. wallet_getCapabilities fusionne les capacites stockees avec une capacite fixe ajoutee cote SDK (gasLimitOverride) et les filtre eventuellement par chaine. Toute une famille de methodes de signature et d envoi (personal_sign, eth_sendTransaction, wallet_sendCalls, wallet_grantPermissions, etc.) passe systematiquement par sendRequestToPopup, donc par le chiffrement du chapitre precedent. Les methodes non reconnues tombent enfin dans un defaut qui relaie la requete brute au RPC de la chaine active via fetchRPCRequest -- utile pour tout ce que le wallet n a pas besoin d approuver, comme eth_getBalance.

Fichier central : `packages/account-sdk/src/sign/base-account/Signer.ts`.

[Chapitre suivant : les sous-comptes et leur financement](07-sub-accounts.md)
