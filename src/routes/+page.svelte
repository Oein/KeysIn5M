<script lang="ts">
	import { onMount } from 'svelte';
	import Title from './title.svelte';
	import { key2num } from './key';

	let result: string[] = [];
	let pressed: null | string = null;
	let disabled = false;
	let renderMouse = false;

	const keyDown = (e: KeyboardEvent) => {
		if (disabled) return;
		e.preventDefault();

		pressed = e.code.toUpperCase().replace('KEY', '');
		renderMouse = false;
	};
	const mouseDown = (e: MouseEvent) => {
		if (disabled) return;
		e.preventDefault();
		e.stopImmediatePropagation();

		renderMouse = true;

		pressed = e.button === 0 ? 'LEFT' : e.button === 1 ? 'MIDDLE' : 'RIGHT';
	};

	const KeyMap: { [key: string]: string[] } = {
		DIGIT1: ['INPUT_SELECT_WEAPON_UNARMED'],
		DIGIT2: ['INPUT_SELECT_WEAPON_MELEE'],
		DIGIT3: ['INPUT_SELECT_WEAPON_SHOTGUN'],
		DIGIT4: ['INPUT_SELECT_WEAPON_HEAVY'],
		DIGIT5: ['INPUT_SELECT_WEAPON_SPECIAL'],
		DIGIT6: ['INPUT_SELECT_WEAPON_HANDGUN'],
		DIGIT7: ['INPUT_SELECT_WEAPON_SMG'],
		DIGIT8: ['INPUT_SELECT_WEAPON_AUTO_RIFLE'],
		DIGIT9: ['INPUT_SELECT_WEAPON_SNIPER'],
		V: [
			'INPUT_NEXT_CAMERA',
			'INPUT_SCRIPT_SELECT',
			'INPUT_REPLAY_TOGGLETIPS',
			'INPUT_REPLAY_TIMELINE_PLACE_CLIP'
		],
		S: [
			'INPUT_SCRIPTED_FLY_UD',
			'INPUT_MOVE_UD',
			'INPUT_MOVE_DOWN_ONLY',
			'INPUT_VEH_BRAKE',
			'INPUT_VEH_HOTWIRE_RIGHT',
			'INPUT_VEH_FLY_THROTTLE_DOWN',
			'INPUT_VEH_SUB_THROTTLE_DOWN',
			'INPUT_VEH_PUSHBIKE_REAR_BRAKE',
			'INPUT_PARACHUTE_PITCH_UD',
			'INPUT_PARACHUTE_PITCH_DOWN_ONLY',
			'INPUT_FRONTEND_AXIS_Y',
			'INPUT_SCRIPT_LEFT_AXIS_Y',
			'INPUT_SCRIPT_PAD_DOWN',
			'INPUT_MOVE_UP',
			'INPUT_MOVE_DOWN',
			'INPUT_REPLAY_RECORD'
		],
		D: [
			'INPUT_SCRIPTED_FLY_LR',
			'INPUT_MOVE_LR',
			'INPUT_MOVE_RIGHT_ONLY',
			'INPUT_VEH_MOVE_LR',
			'INPUT_VEH_MOVE_RIGHT_ONLY',
			'INPUT_VEH_FLY_YAW_RIGHT',
			'INPUT_VEH_SUB_TURN_HARD_RIGHT',
			'INPUT_PARACHUTE_TURN_LR',
			'INPUT_PARACHUTE_TURN_RIGHT_ONLY',
			'INPUT_FRONTEND_AXIS_X',
			'INPUT_SCRIPT_LEFT_AXIS_X',
			'INPUT_SCRIPT_PAD_RIGHT',
			'INPUT_MOVE_LEFT',
			'INPUT_MOVE_RIGHT',
			'INPUT_VEH_MOVE_LEFT',
			'INPUT_VEH_MOVE_RIGHT',
			'INPUT_VEH_HYDRAULICS_CONTROL_RIGHT',
			'INPUT_VEH_HYDRAULICS_CONTROL_UD'
		],
		PAGEUP: ['INPUT_SCRIPTED_FLY_ZUP', 'INPUT_FRONTEND_RT', 'INPUT_REPLAY_CAMERAUP'],
		PAGEDOWN: ['INPUT_SCRIPTED_FLY_ZDOWN', 'INPUT_FRONTEND_LT', 'INPUT_REPLAY_CAMERADOWN'],
		ALTLEFT: ['INPUT_CHARACTER_WHEEL'],
		Z: ['INPUT_MULTIPLAYER_INFO', 'INPUT_HUD_SPECIAL'],
		SHIFTLEFT: [
			'INPUT_SPRINT',
			'INPUT_VEH_MOVE_UP_ONLY',
			'INPUT_VEH_SUB_ASCEND',
			'INPUT_PARACHUTE_PRECISION_LANDING',
			'INPUT_FRONTEND_LS',
			'INPUT_CREATOR_MENU_TOGGLE',
			'INPUT_VEH_HYDRAULICS_CONTROL_UP',
			'INPUT_VEH_FLY_BOOST'
		],
		SPACE: [
			'INPUT_SKIP_CUTSCENE',
			'INPUT_JUMP',
			'INPUT_DIVE',
			'INPUT_VEH_HANDBRAKE',
			'INPUT_VEH_JUMP',
			'INPUT_MELEE_BLOCK',
			'INPUT_CELLPHONE_EXTRA_OPTION',
			'INPUT_FRONTEND_X',
			'INPUT_FRONTEND_ENDSCREEN_EXPAND',
			'INPUT_CREATOR_ACCEPT',
			'INPUT_REPLAY_PAUSE',
			'INPUT_REPLAY_PREVIEW',
			'INPUT_REPLAY_PREVIEW_AUDIO',
			'INPUT_VEH_PARACHUTE'
		],
		F: [
			'INPUT_ENTER',
			'INPUT_ARREST',
			'INPUT_VEH_EXIT',
			'INPUT_PARACHUTE_DETACH',
			'INPUT_CELLPHONE_CAMERA_DOF',
			'INPUT_CREATOR_RS',
			'INPUT_PARACHUTE_DEPLOY'
		],
		C: [
			'INPUT_LOOK_BEHIND',
			'INPUT_VEH_LOOK_BEHIND',
			'INPUT_CREATOR_RT',
			'INPUT_REPLAY_TOGGLETIME',
			'INPUT_REPLAY_TIMELINE_DUPLICATE_CLIP'
		],
		ARROWUP: ['INPUT_PHONE', 'INPUT_CELLPHONE_UP', 'INPUT_FRONTEND_UP', 'INPUT_REPLAY_FFWD'],
		B: ['INPUT_SPECIAL_ABILITY_SECONDARY', 'INPUT_REPLAY_STARTPOINT'],
		W: [
			'INPUT_MOVE_UP_ONLY',
			'INPUT_VEH_ACCELERATE',
			'INPUT_VEH_HOTWIRE_LEFT',
			'INPUT_VEH_FLY_THROTTLE_UP',
			'INPUT_VEH_SUB_THROTTLE_UP',
			'INPUT_VEH_PUSHBIKE_PEDAL',
			'INPUT_PARACHUTE_PITCH_UP_ONLY',
			'INPUT_SCRIPT_PAD_UP'
		],
		A: [
			'INPUT_MOVE_LEFT_ONLY',
			'INPUT_VEH_MOVE_LEFT_ONLY',
			'INPUT_VEH_FLY_YAW_LEFT',
			'INPUT_VEH_SUB_TURN_HARD_LEFT',
			'INPUT_PARACHUTE_TURN_LEFT_ONLY',
			'INPUT_SCRIPT_PAD_LEFT',
			'INPUT_VEH_HYDRAULICS_CONTROL_LEFT'
		],
		CONTROLLEFT: [
			'INPUT_DUCK',
			'INPUT_VEH_MOVE_UD',
			'INPUT_VEH_MOVE_DOWN_ONLY',
			'INPUT_VEH_SUB_DESCEND',
			'INPUT_SCRIPT_RLEFT',
			'INPUT_VEH_MOVE_UP',
			'INPUT_VEH_MOVE_DOWN',
			'INPUT_REPLAY_CTRL',
			'INPUT_VEH_HYDRAULICS_CONTROL_DOWN',
			'INPUT_VEH_HYDRAULICS_CONTROL_LR',
			'INPUT_FRONTEND_RS'
		],
		TAB: [
			'INPUT_SELECT_WEAPON',
			'INPUT_FRONTEND_RUP',
			'INPUT_FRONTEND_Y',
			'INPUT_FRONTEND_LEADERBOARD',
			'INPUT_REPLAY_SNAPMATIC_PHOTO'
		],
		E: [
			'INPUT_PICKUP',
			'INPUT_TALK',
			'INPUT_CONTEXT',
			'INPUT_WEAPON_SPECIAL_TWO',
			'INPUT_VEH_HORN',
			'INPUT_VEH_GRAPPLING_HOOK',
			'INPUT_VEH_FLY_VERTICAL_FLIGHT_MODE',
			'INPUT_PARACHUTE_BRAKE_RIGHT',
			'INPUT_CELLPHONE_CAMERA_SELFIE',
			'INPUT_FRONTEND_RB',
			'INPUT_VEH_CAR_JUMP',
			'INPUT_VEH_ROCKET_BOOST',
			'INPUT_VEH_FLY_BOMB_BAY',
			'INPUT_VEH_FLY_COUNTER'
		],
		BRACKETLEFT: [
			'INPUT_SNIPER_ZOOM',
			'INPUT_SNIPER_ZOOM_OUT_ONLY',
			'INPUT_SNIPER_ZOOM_OUT_SECONDARY',
			'INPUT_VEH_SELECT_PREV_WEAPON',
			'INPUT_VEH_FLY_SELECT_PREV_WEAPON',
			'INPUT_SNIPER_ZOOM_IN',
			'INPUT_SNIPER_ZOOM_OUT',
			'INPUT_SNIPER_ZOOM_IN_ALTERNATE',
			'INPUT_SNIPER_ZOOM_OUT_ALTERNATE',
			'INPUT_REPLAY_CYCLEMARKERLEFT'
		],
		BRACKETRIGHT: [
			'INPUT_SNIPER_ZOOM_IN_ONLY',
			'INPUT_SNIPER_ZOOM_IN_SECONDARY',
			'INPUT_FRONTEND_RIGHT_AXIS_X',
			'INPUT_REPLAY_CYCLEMARKERRIGHT'
		],
		Q: [
			'INPUT_COVER',
			'INPUT_CONTEXT_SECONDARY',
			'INPUT_VEH_RADIO_WHEEL',
			'INPUT_VEH_PUSHBIKE_FRONT_BRAKE',
			'INPUT_MELEE_ATTACK_HEAVY',
			'INPUT_PARACHUTE_BRAKE_LEFT',
			'INPUT_FRONTEND_LB',
			'INPUT_MELEE_ATTACK2'
		],
		R: [
			'INPUT_RELOAD',
			'INPUT_VEH_CIN_CAM',
			'INPUT_MELEE_ATTACK_LIGHT',
			'INPUT_CREATOR_LS',
			'INPUT_MELEE_ATTACK1',
			'INPUT_REPLAY_RESTART'
		],
		G: [
			'INPUT_DETONATE',
			'INPUT_THROW_GRENADE',
			'INPUT_VEH_FLY_UNDERCARRIAGE',
			'INPUT_CELLPHONE_CAMERA_GRID'
		],
		F9: ['INPUT_DROP_WEAPON'],
		F10: ['INPUT_DROP_AMMO'],
		X: [
			'INPUT_VEH_DUCK',
			'INPUT_VEH_DROP_PROJECTILE',
			'INPUT_VEH_FLY_DUCK',
			'INPUT_PARACHUTE_SMOKE',
			'INPUT_CELLPHONE_CAMERA_EXPRESSION',
			'INPUT_CREATOR_LT',
			'INPUT_REPLAY_TIMELINE_PICKUP_CLIP',
			'INPUT_VEH_HYDRAULICS_CONTROL_TOGGLE',
			'INPUT_VEH_MELEE_HOLD',
			'INPUT_VEH_BIKE_WINGS',
			'INPUT_VEH_TRANSFORM'
		],
		H: ['INPUT_VEH_HEADLIGHT', 'INPUT_VEH_ROOF', 'INPUT_VEH_SHUFFLE', 'INPUT_REPLAY_HIDEHUD'],
		PERIOD: ['INPUT_VEH_NEXT_RADIO'],
		COMMA: ['INPUT_VEH_PREV_RADIO'],
		EQUAL: ['INPUT_VEH_NEXT_RADIO_TRACK'],
		MINUS: ['INPUT_VEH_PREV_RADIO_TRACK'],
		NUMPADSUBTRACT: ['INPUT_VEH_CINEMATIC_UP_ONLY', 'INPUT_REPLAY_FOVDECREASE'],
		NUMPADADD: ['INPUT_VEH_CINEMATIC_DOWN_ONLY', 'INPUT_REPLAY_FOVINCREASE'],
		NUMPAD6: [
			'INPUT_VEH_FLY_ROLL_LR',
			'INPUT_VEH_FLY_ROLL_RIGHT_ONLY',
			'INPUT_VEH_SUB_TURN_LR',
			'INPUT_VEH_SUB_TURN_RIGHT_ONLY'
		],
		NUMPAD4: ['INPUT_VEH_FLY_ROLL_LEFT_ONLY', 'INPUT_VEH_SUB_TURN_LEFT_ONLY'],
		NUMPAD5: [
			'INPUT_VEH_FLY_PITCH_UD',
			'INPUT_VEH_FLY_PITCH_DOWN_ONLY',
			'INPUT_VEH_SUB_PITCH_UD',
			'INPUT_VEH_SUB_PITCH_DOWN_ONLY'
		],
		NUMPAD8: ['INPUT_VEH_FLY_PITCH_UP_ONLY', 'INPUT_VEH_SUB_PITCH_UP_ONLY'],
		NUMPAD7: ['INPUT_VEH_FLY_SELECT_TARGET_LEFT'],
		NUMPAD9: ['INPUT_VEH_FLY_SELECT_TARGET_RIGHT'],
		HELP: ['INPUT_VEH_FLY_ATTACK_CAMERA'],
		CAPSLOCK: ['INPUT_VEH_PUSHBIKE_SPRINT', 'INPUT_SPECIAL_ABILITY_PC', 'INPUT_FRONTEND_SELECT'],
		F5: ['INPUT_SELECT_CHARACTER_MICHAEL', 'INPUT_REPLAY_SAVE', 'INPUT_REPLAY_TIMELINE_SAVE'],
		F6: ['INPUT_SELECT_CHARACTER_FRANKLIN'],
		F7: ['INPUT_SELECT_CHARACTER_TREVOR'],
		F8: ['INPUT_SELECT_CHARACTER_MULTIPLAYER'],
		F3: ['INPUT_SAVE_REPLAY_CLIP'],
		ARROWDOWN: ['INPUT_CELLPHONE_DOWN', 'INPUT_FRONTEND_DOWN', 'INPUT_REPLAY_REWIND'],
		ARROWLEFT: ['INPUT_CELLPHONE_LEFT', 'INPUT_FRONTEND_LEFT', 'INPUT_REPLAY_BACK'],
		ARROWRIGHT: ['INPUT_CELLPHONE_RIGHT', 'INPUT_FRONTEND_RIGHT', 'INPUT_REPLAY_ADVANCE'],
		DELETE: [
			'INPUT_CELLPHONE_OPTION',
			'INPUT_FRONTEND_DELETE',
			'INPUT_CREATOR_DELETE',
			'INPUT_REPLAY_MARKER_DELETE',
			'INPUT_REPLAY_CLIP_DELETE'
		],
		L: ['INPUT_CELLPHONE_CAMERA_FOCUS_LOCK'],
		ENTER: [
			'INPUT_FRONTEND_RDOWN',
			'INPUT_FRONTEND_ENDSCREEN_ACCEPT',
			'INPUT_SKIP_CUTSCENE',
			'INPUT_FRONTEND_ACCEPT',
			'INPUT_CELLPHONE_SELECT',
			"'INPUT_FRONTEND_ACCEPT'"
		],
		BACKSPACE: ['INPUT_FRONTEND_RRIGHT', 'INPUT_CELLPHONE_CANCEL', 'INPUT_FRONTEND_CANCEL'],
		P: ['INPUT_FRONTEND_PAUSE'],
		ESCAPE: [
			'INPUT_FRONTEND_PAUSE_ALTERNATE',
			'INPUT_REPLAY_TOGGLE_TIMELINE',
			'INPUT_CELLPHONE_CANCEL',
			'INPUT_FRONTEND_CANCEL'
		],
		NUMPADENTER: ['INPUT_FRONTEND_ACCEPT'],
		HOME: ['INPUT_FRONTEND_SOCIAL_CLUB', 'INPUT_FRONTEND_SOCIAL_CLUB_SECONDARY'],
		BACKQUOTE: ['INPUT_ENTER_CHEAT_CODE'],
		M: ['INPUT_INTERACTION_MENU', 'INPUT_REPLAY_NEWMARKER'],
		T: ['INPUT_MP_TEXT_CHAT_ALL', 'INPUT_REPLAY_TOOLS'],
		Y: ['INPUT_MP_TEXT_CHAT_TEAM'],
		N: ['INPUT_PUSH_TO_TALK', 'INPUT_REPLAY_ENDPOINT'],
		F1: ['INPUT_REPLAY_START_STOP_RECORDING'],
		F2: ['INPUT_REPLAY_START_STOP_RECORDING_SECONDARY'],
		U: ['INPUT_REPLAY_SCREENSHOT'],
		K: ['INPUT_REPLAY_SHOWHOTKEY'],
		F11: ['INPUT_SWITCH_VISOR']
	};

	const MouseMap: { [key: string]: string[] } = {
		LEFT: [
			'INPUT_SKIP_CUTSCENE',
			'INPUT_ATTACK',
			'INPUT_VEH_ATTACK',
			'INPUT_VEH_PASSENGER_ATTACK',
			'INPUT_VEH_MOUSE_CONTROL_OVERRIDE',
			'INPUT_VEH_FLY_MOUSE_CONTROL_OVERRIDE',
			'INPUT_VEH_SUB_MOUSE_CONTROL_OVERRIDE',
			'INPUT_MELEE_ATTACK_ALTERNATE',
			'INPUT_SCRIPT_RDOWN',
			'INPUT_SCRIPT_RT',
			'INPUT_CURSOR_ACCEPT',
			'INPUT_ATTACK2',
			'INPUT_VEH_DRIVE_LOOK',
			'INPUT_VEH_MELEE_LEFT',
			'INPUT_PARACHUTE_DEPLOY',
			'INPUT_CELLPHONE_SELECT'
		],
		RIGHT: [
			'INPUT_AIM',
			'INPUT_VEH_AIM',
			'INPUT_VEH_ATTACK2',
			'INPUT_VEH_PASSENGER_AIM',
			'INPUT_VEH_FLY_ATTACK',
			'INPUT_SCRIPT_RUP',
			'INPUT_SCRIPT_RRIGHT',
			'INPUT_CURSOR_CANCEL',
			'INPUT_VEH_DRIVE_LOOK2',
			'INPUT_VEH_FLY_ATTACK2',
			'INPUT_VEH_MELEE_RIGHT',
			'INPUT_CELLPHONE_CANCEL'
		]
	};

	onMount(() => {
		window.addEventListener('keydown', keyDown);
		window.addEventListener('mousedown', mouseDown);

		return () => {
			window.removeEventListener('keydown', keyDown);
			window.removeEventListener('mousedown', mouseDown);
		};
	});

	const dontGoUP = (e: Event) => {
		disabled = true;

		e.stopImmediatePropagation();
		e.stopPropagation();
	};
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="wrp" on:mousedown={() => (disabled = false)}>
	<Title />
	{#if pressed == null}
		<div class="desc">Press any keys or mouse button to view in code.</div>
	{:else}
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<div class="result" on:mousedown={dontGoUP} on:keydown={dontGoUP}>
			<div class="desc">
				You pressed <span class="descBold">{pressed}</span>
			</div>
			<div class="resbox">
				{#if renderMouse}
					{#if typeof MouseMap[pressed] != 'undefined'}
						<div class="tit">
							There are <span class="bold">{MouseMap[pressed].length}</span> keys you can use.
						</div>

						{#each MouseMap[pressed] as key}
							<div class="k">
								- {key} <span class="small">({key2num[key]})</span>
							</div>
						{/each}
					{:else}
						<div class="tit">
							There are <span class="bold">4</span> keys you can use.
						</div>

						<div class="stitle">For press</div>
						<div class="k">
							- INPUT_PHONE <span class="small">({key2num['INPUT_PHONE']})</span>
						</div>
						<div class="k">
							- INPUT_MAP_POI <span class="small">({key2num['INPUT_MAP_POI']})</span>
						</div>
						<div class="stitle">For scroll up</div>
						<div class="k">
							- INPUT_VEH_CINEMATIC_UP_ONLY <span class="small"
								>({key2num['INPUT_VEH_CINEMATIC_UP_ONLY']})</span
							>
						</div>
						<div class="stitle">For scroll down</div>
						<div class="k">
							- INPUT_MAP_POI <span class="small">({key2num['INPUT_MAP_POI']})</span>
						</div>
					{/if}
				{:else if typeof KeyMap[pressed] != 'undefined'}
					<div class="tit">
						There are <span class="bold">{KeyMap[pressed].length}</span> keys you can use.
					</div>

					{#each KeyMap[pressed] as key}
						<div class="k">
							- {key} <span class="small">({key2num[key]})</span>
						</div>
					{/each}
				{:else}
					<div class="tit">
						There are <span class="bold">No</span> keys you can use.
					</div>
				{/if}
			</div>
		</div>
	{/if}
</div>
<div class="pos">
	{#if disabled}
		Press outside of box to start observe your input.
	{:else}
		Now observing your key.
	{/if}
</div>

<style>
	.pos {
		position: fixed;
		left: 0.5rem;
		bottom: 0.5rem;
		color: #4d6772;
	}
	.wrp {
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		gap: 1rem;
	}
	.desc {
		color: #4d6772;
		font-size: 1.5rem;
	}
	.descBold {
		color: #fff;
		font-weight: 700;
	}
	.result {
		width: 100%;
		box-sizing: border-box;

		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		align-items: center;
		max-width: 800px;
	}
	.resbox {
		margin-top: 1rem;
		max-width: 800px;
		width: 100%;
		box-sizing: border-box;

		padding: 1rem;
		border-radius: 1rem;
		background: #2e3c43;

		font-size: 1rem;
	}
	.tit {
		text-align: start;
		font-size: 1.25rem;
		margin-bottom: 0.5rem;
		padding-bottom: 0.5rem;
		border-bottom: 1px solid rgba(255, 255, 255, 0.3);
	}
	.bold {
		font-weight: 700;
		padding: 0px 0.5rem;
		border-bottom: 1px solid #fff;
	}
	.k {
		font-size: 0.9em;
	}
	.small {
		font-size: 0.9em;
		color: #4d6772;
	}
	.stitle {
		font-size: 600;
		margin-top: 0.5rem;
	}
</style>
