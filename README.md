# KamailioAPNS

#### update devicetoken

```
route[REGISTRAR] {
	if (!is_method("REGISTER")) return;

	if(isflagset(FLT_NATS)) {
		setbflag(FLB_NATB);
#!ifdef WITH_NATSIPPING
		# do SIP NAT pinging
		setbflag(FLB_NATSIPPING);
#!endif
	}
	if (!save("location")) {
		sl_reply_error();
	}
	route(PUSHJSON);
    # update devicetoken
	#!ifdef WITH_APPPYTHON3
		python_exec("PushNotification","{'method':'register','sipNum':'$tU','token':'$(ct{param.value,pn-tok})'}");
	#!endif
	exit;
}

```

#### push notifications

> The user handles the offline status to push notifications.

```
if (is_method("INVITE")) {
		
		# setflag(FLT_ACC); # do accounting
		
		if (!lookup("location")) {
			# push notifications
			#!ifdef WITH_APPPYTHON3
			python_exec("PushNotification","{'method':'INVITE','sipNum':'$rU'}");
		#!endif
			send_reply("100","Trying - APNS");
			route(PUSHASYNC);
		}
		# else{
		# 	t_relay();
		# 	ts_store();
		# 	$sht(vtp=>stored::$rU) = 1;
		# }
		
		
	}

```

> Android for the same reason,python script processing.

