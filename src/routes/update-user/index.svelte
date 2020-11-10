<script>
    import { onMount } from 'svelte';
    import { goto, stores } from "@sapper/app";
    import ListErrors from '../_components/ListErrors.svelte';
    import CountriesList from '../_components/CountriesList.svelte';
    import UpdateField from '../_components/UpdateField.svelte'
    import * as api from 'api.js';

    const { session } = stores();

    let errors = null;
    errors = 'Messages are displayed here';


    let firstName = '';
    let lastName = '';
    let fullName = '';
    let fullNameArr;
    let email = '';
    let phoneNumber;
    let whatsAppNumber;
    let country = '356';
    let state;
    let city;
    let preferredLanguages,preferredLanguagesArr,  language1, language2, language3;
    let now = new Date(), month, day, year;
    let birthDate;
    let gender;
    let occupation;
    let organization;
    let files;
    let telegramId; 
    //telegram Id is connected to   slack user id
    let socialMediaLinks ;
    let shareOptions = [
		{ id: 1, text: `Share with connected learners` },
        { id: 2, text: `Don't share with anyone` }
    ];
    let connectionOptions = [
        { id : 1, text: 'Private account'},
        { id : 2, text: 'Public account'}
    ];
    let connectionPrivacy;
    function submit(event) {
         console.log( ' Update Called');
        preferredLanguages = language1 + "|" +  language2 + "|" + language3;
        fullName= firstName          + " " +  lastName; 
// todo add update for phonenr,whatsnr,language,country after api changes
        const response =  api.put("user", {
        name: fullName,
     location: city,
        occupation  : occupation,
        organization: organization,
slack_username: telegramId,
social_media_links:socialMediaLinks 
        },
        $session.user.access_token    
        );
        errors = 'Your changes have been updated';


    }
    
    onMount(()=> {
        api.get(
		"user",
		$session.user.access_token
		).then((user)=> {
            fullName = user.name;
            email = user.email;
            // todo connect after API changes phoneNumber= user.phonenumber;
                // todo connect after API changes whatsAppNumber =user.whatsappnumber ;
     country = '356';
    city = user.location;
    // todo After API changesbirthDate;
    // After API Changes gender;
    occupation=user.occupation;
    organization = user.organization;
    files =user.photo_url ;
    telegramId= user.slack_username;
    // After API changes  connectionPrivacy;
socialMediaLinks =user.social_media_links;
preferredLanguages=user.language;

console.log(fullName);
        fullNameArr = fullName.split(" ");
        
        firstName= fullNameArr[0]       ;
        lastName= fullNameArr[1];

        preferredLanguagesArr = preferredLanguages.split("|");

        language1 = preferredLanguagesArr[0];
        language2 = preferredLanguagesArr[1];
        language3 = preferredLanguagesArr[2];
        
        })
        month = '' + (now.getMonth() + 1),
        day = '' + now.getDate(),
        year = now.getFullYear();

        if (month.length < 2) 
            month = '0' + month;
        if (day.length < 2) 
            day = '0' + day;

        birthDate = [year, month, day].join('-');
	})
</script>

<style>
.desc-parent{
    margin: 12px 0 0 0;
}
.desc-text{
    margin: 0 0 5px 0;
}
.red-text{
    color: red;
}
</style>

<svelte:head>
	<title>Update User</title>
</svelte:head>

<div class="update-user-page">
    <div class="container page">
		<div class="row">
            <div class="col-md-6 offset-md-3 col-xs-12">
                <h1 class="text-xs-center">Update Profile</h1>

                <ListErrors {errors}/>

                <form on:submit|preventDefault={submit}>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={firstName}
                            iconClass="person-outline"
                            descText="First Name"
                            inputType="text"
                        />
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={lastName}
                            iconClass="person-outline"
                            descText="Last Name"
                            inputType="text"
                        />
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={email}
                            iconClass="mail-outline"
                            descText="E-mail"
                            inputType="email"
                        />
                        <div class="row">
                            <div class="col-sm-6 offset-sm-6">
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={phoneNumber}
                            iconClass="call-outline"
                            descText="Phone Number"
                            inputType="tel"
                        />
                        <div class="row">
                            <div class="col-sm-6 offset-sm-6">
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={whatsAppNumber}
                            iconClass="logo-whatsapp"
                            descText="WhatsApp Number"
                            inputType="tel"
                        />
                        <div class="row">
                            <div class="col-sm-6 offset-sm-6">
                            </div>
                        </div>
                    </fieldset>

                                       <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={telegramId}
                            iconClass="person-outline"
                            descText="Telegram User ID"
                            inputType="text"
                        />
                    </fieldset>
 
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-4">
                                <p class="desc-parent">
                                    <ion-icon name="location-outline"></ion-icon>
                                    <span class="detail-desc">Country</span>
                                </p>
                            </div>
                            <div class="col-sm-8">
                                <CountriesList bind:value={country} />
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={state}
                            iconClass="location-outline"
                            descText="State/ Province"
                            inputType="text"
                        />
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={city}
                            iconClass="location-outline"
                            descText="City/ Town"
                            inputType="text"
                        />
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-3">
                                <p class="desc-parent">
                                    <ion-icon name="language-outline"></ion-icon>
                                    <span class="detail-desc">Language</span>
                                </p>
                            </div>
                            <div class="col-sm-3">
                                <input class="form-control form-control-md input-desc" type="text" placeholder="Language 1" bind:value={language1}>
                            </div>
                            <div class="col-sm-3">
                                <input class="form-control form-control-md input-desc" type="text" placeholder="Language 2" bind:value={language2}>
                            </div>
                            <div class="col-sm-3">
                                <input class="form-control form-control-md input-desc" type="text" placeholder="Language 3" bind:value={language3}>
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-4">
                                <p class="desc-parent">
                                    <ion-icon name="shield-outline"></ion-icon>
                                    <span class="detail-desc">Connection privacy</span>
                                </p>
                            </div>
                            <div class="col-sm-8">
                                <select class="form-control form-control-md" value={connectionPrivacy}>
                                    {#each connectionOptions as option}
                                        <option value={option}>
                                            {option.text}
                                        </option>
                                    {/each}
                                </select>
                            </div>
                        </div>
                    </fieldset>
                    <hr />
                    <p class="desc-text">Private user data</p>
                    <p class="desc-text red-text">Below field data will not be shared</p>
                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-6">
                                <p class="desc-parent">
                                    <ion-icon name="calendar-outline"></ion-icon>
                                    <span class="detail-desc">Birth date</span>
                                </p>
                            </div>
                            <div class="col-sm-6">
                                <input class="form-control form-control-md input-desc" type="date" bind:value={birthDate}>
                            </div>
                        </div>
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={gender}
                            iconClass="male-female-outline"
                            descText="Gender"
                            inputType="text"
                        />
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={occupation}
                            iconClass="briefcase-outline"
                            descText="Occupation"
                            inputType="text"
                        />
                    </fieldset>
<fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={organization}
                            iconClass="briefcase-outline"
                            descText="Organization"
                            inputType="text"
                        />
                    </fieldset>
                    


                    <fieldset class="form-group">
                        <div class="row">
                            <div class="col-sm-6">
                                <p class="desc-parent">
                                    <ion-icon name="camera-outline"></ion-icon>
                                    <span class="detail-desc">Photo</span>
                                </p>
                            </div>
                            <div class="col-sm-6">
                                <input class="form-control form-control-md input-desc" type="file" bind:files accept="image/*">
                            </div>
                        </div>
                    </fieldset>
                    <button class="btn btn-md btn-primary pull-xs-right">
						Update
					</button>
                </form>
            </div>
        </div>
    </div>
</div>