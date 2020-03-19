The moderator system can cascade up to a limit of 2.

For example, consider the following below, note that each name in capitals is equivalent to an "account" on Discussions. Moderation groups are effectively accounts as well.

ALICE
	- b.posts
		- #eos
			- {p1, p2}
	- b.users
		- all
			- { DAVE }
	- delegated
		- all
			- { NOVUSPHERE }
		- #eoscafe
			- { EOSCAFETEAM }
NOVUSPHERE
	- delegated
		- all
			- { NSTEAM }
		- #eosgo
			- { EOSGOTEAM }

NSTEAM
	- delegated
		- all
			- { JACUQES, PAUL }

EOSGOTEAM
	- delegated
		- all
			- { JOE }

EOSCAFETEAM
	- delegated
		- all
			- { CARLOS }

JACQUES
	- b.posts
		- #eos
			- { p1, p2, p3, p4 }
	- b.users
		- all
			- { CAROL }
	- delegated
		- all
			- { ASPHYXIA }

PAUL
	- b.posts
		- #eos
			- { p3, p5}
		- #abc
			- { p10, p11 }

JOE
	- b.posts
		- #eos
			- { p6, p7 }
		- #eosgo
			- { p20, p21 }
	- b.users
		- all
			- { LENNY }
		- #eos
			- { KYLE }

CARLOS
	- b.posts
		- #eoscafe
			- { p30 }


---- End Result ----

ALICE
	- b.posts
		- #eos
			- { p1, p2, p3, p4, p5 }
		- #abc
			- { p10, p11 }
		- #eosgo
			- { p20, p21 }
		- #eoscafe
			- { p30 }
	- b.users
		- all
			- { DAVE, CAROL }
		- #eosgo
			- { LENNY }

---- End Result ----

Note that ALICE does not inherit any settings from ASPHYXIA as this is beyond the cascade limit as shown below:
	ALICE -> all NOVUSPHERE -> all NSTEAM -> all JACQUES -> all ASPHYXIA -> ...

Notice that ALICE does get JACQUES's b.user "all CAROL". This is allowed because:
	ALICE -> all NOVUSPHERE -> all NSTEAM -> all JACQUES -> b.user all CAROL
		
Notice that ALICE has b.user "#eosgo LENNY" yet, this is inherited from "all LENNY" from JOE. This is because:
	ALICE -> all NOVUSPHERE -> #eosgo EOSGOTEAM -> #eosgo JOE -> b.user all LENNY

"b.user all" can be interpreted as having "b.user #a LENNY", "b.user #b LENNY", and so on. So obviously, we can get, "b.user #eosgo LENNY".

Simialrly to above, notice "b.user #eos KYLE" is not inherited from JOE since ALICE only inherit "#eosgo" related settings from JOE.

