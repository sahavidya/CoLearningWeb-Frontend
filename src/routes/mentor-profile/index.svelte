<script>
  import { stores } from "@sapper/app";
  import * as api from "api.js";

  const { session, page } = stores();

  let mentorid,
    name,
    isguide,
    city,
    bio,
    interests,
    skills;

  let userData;

  const languages = "English, Hindi, Marathi";

  console.log("Mentor Profile Mentor id:" + $page.query.mentorId);

  async function getData() {
    userData = await api.get(
      "users/" + $page.query.mentorId,
      $session.user.access_token
    );


    console.log("User name:" +  userData.username);
    console.log("name:" +  userData.name);
    console.log("bio:" +  userData.bio);


    //console.log("User data:" + userData);
    mentorid = userData.id;
    console.log("User id:" + mentorid);
    name = userData.name;
    isguide = userData.available_to_mentor;
    skills = userData.skills;
    interests = userData.interests;
    city = userData.location;
    bio = userData.bio;
    
    console.log("user guiding skills " + guidingSkills);
    console.log("first name " + firstname);
    skills = guidingSkills;
  }
  getData();

  console;
</script>

<style>
  .aoe-span {
    color: orange;
    font-weight: 600;
  }
</style>

<svelte:head>
  <title>Mentor Profile</title>
</svelte:head>

<div class="request-connect-page">
  <div class="container page">
    <div class="row">
      <div class="col-md-6 offset-md-3 col-xs-12">
        <div class="row">
          <div class="media-left">
            <img
              class="media-object img-circle"
              src="images/temp.jpg"
              alt="Profile"
              width="50"
              height="50" />
            <div class="col-sm-8">
              {name}
              <p>
                (
                <span class={`guide-span ${isguide ? '' : 'no-display'}`}>
                  Guide
                </span>
                )
              </p>
            </div>
          </div>

          <div class="col-sm-8">
            <div class="aoe-container row">
              <div class="aoe-span col-xs-1">AOE:</div>

              <div class="col-xs-10">
                <div>{skills}</div>
              </div>
            </div>
          </div>
          <div class="col-sm-8">
             <div class="aoe-container row">
              <div class="aoe-span col-xs-1">Interests:</div>

              <div class="col-xs-10">
                <div>{interests}</div>
                </div>
            </div>
            <div>{city}</div>
            <div>{languages}</div>
          </div>
        </div>
        <!--div class="row">
          {#if phonenumberprivacy && whatsappnumberprivacy && emailprivacy}
            <p>This is a Private Account</p>
          {:else if !phonenumberprivacy}
            <p>Phone No: {phonenumber}</p>
          {:else if !whatsappnumberprivacy}
            <p>Whatsapp Number: {whatsappnumber}</p>
          {:else if !emailprivacy}
            <p>Email: {email}</p>
          {/if}
        </div>-->
        <div class="row">
          <p>{name}'s Words:</p>
          <p>{bio}</p>
        </div>
        <div class="details-container">
          <a href="/request-connect?mentorId={mentorid}">
            <button type="button" class="btn btn-md btn-primary pull-xs-right">
              Connect with {name}
            </button>
          </a>
        </div>
      </div>

    </div>
  </div>
</div>
