#include<cstdlib>
#include<iostream>
#include<cstring>
/*  This program generates Non-Player Characters for the
	Pathfinder tabletop game.  User inputs the number of
	NPC's desired and their experience level, program then
	produces complete info blocks for each character.  
	Program written by Alex Morrese.
*/

/*
The MIT License (MIT)

Copyright (c) 2013 Alexander Morrese

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
*/

using namespace std;

enum role {WARRIOR,SCOUT,EXPERT,CASTER,LEADER,HIRELING};  //Possible character jobs
int level;		//party level

int max(int,int);

class Character		//each character established as new object
{
private:
	int stats[6],saves[3],skills[33];
	int hp,ac,melee,ranged,spd,init,cmb,cmd;
	role job;
	char name[12],race[10];
	void rollstats();
	void jobselect();
	void levelup();
	void skillselect(int,int[],int);
	void feats(int);
	void warrior();
	void scout();
	void expert();
	void caster();
	void leader();
	void hireling();
	void raceselect();
	void dwarf();
	void elf();
	void gnome();
	void helf();
	void horc();
	void halfling();
	void human();
	void statboost(int);
	void armor();
	void weapons();
public:
	Character();
	void printchar();
};

Character::Character()				//creates a new character by, in order:
{
	rollstats();					//rolls character's basic abilities
	jobselect();					//selects the character's job
	init=(stats[2]-10)/2; spd=6;	//sets character's initiative and speed
	levelup();						//increases character's level
	raceselect();					//selects characters race origin
	saves[0]=saves[0]+(stats[1]-10)/2;	//raises character's saving throws
	saves[1]=saves[1]+(stats[2]-10)/2;	//fortitude, reflex, and will
	saves[2]=saves[2]+(stats[4]-10)/2;
}

void Character::rollstats()		//rolls basic ability scores
{								//Strength, Constitution, Dexterity
	int roll[3];				//Intelligence, Wisdom, Charisma
	int x,y,temp;
	
	for(y=0;y<6;y++)		//rolls and sums 3 6-sided dice for each score
	{
		for(x=0;x<3;x++)
			roll[x]=rand()%6+1;
		stats[y]=roll[0]+roll[1]+roll[2];
	}
}

void Character::jobselect()		//selects character's job based on best attribute
{
	if(stats[5]>=stats[0] && stats[5]>=stats[2] && stats[5]>=stats[3] 
		&& stats[5]>=stats[4] && stats[5]>=12) job=LEADER;	//if charisma best select Leader
	else if(stats[2]>=stats[0] && stats[2]>=stats[3] && stats[2]>=stats[4] 
		&& stats[2]>=stats[5] && stats[2]>=12) job=SCOUT;	//if Dexterity best select Scout
	else if(stats[4]>=stats[0] && stats[4]>=stats[2] && stats[4]>=stats[3]
		&& stats[4]>=stats[5] && stats[4]>=12) job=CASTER;	//if Wisdom best select Caster
    else if(stats[3]>=stats[0] && stats[3]>=stats[2] && stats[3]>=stats[4] 
		&& stats[3]>=stats[5] && stats[3]>=12) job=EXPERT;	//if Intelligence select Expert
    else if(stats[0]>=12) job=WARRIOR;		//if Strength adequate select Warrior
	else job=HIRELING;	//if no attribute adequate select Hireling
}

void Character::levelup()	//sets and improves character abilities by job and level
{
int x,hd;

switch (job)		//calls appropriate function for character job
	{
	case WARRIOR: warrior(); hd=10; break;	//job functions set class skill list
	case SCOUT: scout(); hd = 8; break;		//number of skills allowed by job
	case EXPERT: expert(); hd=8; break;		//basic saving throws
    case LEADER: leader(); hd=8; break;		//and base attack bonus
	case CASTER: caster(); hd=6; break;
	default: hireling(); hd=6;
	}

statboost(level/4);		//improves core ability every four levels
feats(level);			//selects feats for the character

hp=hd;
for(x=1;x<level;x++)	//raises character hit points
	hp=hp+max(1,rand()%hd+1+(stats[1]-10)/2);
}

void Character::warrior()	//warrior has good fortitude save
{							//six job skills and selects 2+
	int s,num=6;
	int jobskills[6]={3,8,10,14,20,32};
	saves[0]=level/2+2;saves[1]=level/3;saves[2]=level/3;
	melee=level+(stats[0]-10)/2+1; ranged=level+(stats[2]-10)/2;
	ac=10; 
	cmd=level+(stats[0]-10)/2; cmb=10+cmd+(stats[2]-10)/2;
	s=max(1,2+(stats[3]-10)/2);
	skillselect(s,jobskills,num);
}

void Character::scout()	//scout has good reflex save
{						//twelve job skills and selects 10+
	int s, num=12;
	int jobskills[12]={3,8,9,10,12,14,18,19,20,25,28,32};
	saves[0]=level/3;saves[1]=level/2+2;saves[2]=level/3;
	melee=level*3/4+(stats[2]-10)/2; ranged=melee;
	ac=10+(stats[2]-10)/2;
	cmd=level+(stats[0]-10)/2; cmb=10+cmd+(stats[2]-10)/2;
	s=max(1,6+(stats[3]-10)/2);
	skillselect(s,jobskills,num);
}

void Character::caster()	//caster has good will save
{							//15 job skills and selects 2+
	int bab, s, num=15;
	int jobskills[]={8,9,17,19,22,23,24,25,26,27,28,29,30,31,32};
	saves[0]=level/3;saves[1]=level/3;saves[2]=level/2+2;
	bab=level/2+level/5+1;
	melee=bab+(stats[0]-10)/2; ranged=bab+(stats[2]-10)/2;
	ac=10+(stats[2]-10)/2;
	cmd=level+(stats[0]-10)/2; cmb=10+cmd+(stats[2]-10)/2;
	s=max(1,2+(stats[3]-10)/2);
	skillselect(s,jobskills,num);
}

void Character::leader()	//leader has good will save
{							//24 job skills and selects 4+
	int s, num=24;
	int jobskills[24]={1,2,4,6,8,10,11,12,13,14,15,19,20,
					22,23,24,25,26,27,28,29,30,31,32};
	saves[0]=level/3;saves[1]=level/3;saves[2]=level/2+2;
	melee=level*3/4+(stats[0]-10)/2; ranged=level*3/4+(stats[2]-10)/2;
	ac=10;
	cmd=level+(stats[0]-10)/2; cmb=10+cmd+(stats[2]-10)/2;
	s=max(1,4+(stats[3]-10)/2);
	skillselect(s,jobskills,num);

	skills[4]+3*(1+level/10);
}

void Character::hireling()	//hireling has no good saves
{							//6 job skills and selects 2+
	int s, x, n, num=6;
	int jobskills[6]={3,8,12,14,20,32};
	saves[0]=level/3;saves[1]=saves[0];saves[2]=saves[0];
	melee=level/2+(stats[0]-10)/2; ranged=level/2+(stats[2]-10)/2;
	ac=10+(stats[2]-10)/2;
	cmd=level+(stats[0]-10)/2; cmb=10+cmd+(stats[2]-10)/2;
	s=max(1,2+(stats[3]-10)/2);
	skillselect(s,jobskills,num);

	x=0;	
	while(x==0)
	{
		n=rand()%33;
		if (skills[n]>=level+3) 
		{skills[n]=skills[n]+3*(1+level/10);x=1;}
	}
}

void Character::expert()	//expert has good will save
{							//and selects any 10 random skills
	int x,n,s,num=33;
	int jobskills[33];
	for (x=0;x<33;x++) jobskills[x]=x;
	saves[0]=level/3;saves[1]=level/3;saves[2]=level/2+2;
	melee=level*3/4+(stats[0]-10)/2;
	ranged=level*3/4+(stats[2]-10)/2;
	ac=10+(stats[2]-10)/2;
	cmd=level+(stats[0]-10)/2; cmb=10+cmd+(stats[2]-10)/2;
	s=6+(stats[3]-10)/2;
	skillselect(s,jobskills,num);

	x=0;	
	while(x==0)
	{
		n=rand()%33;
		if (skills[n]>=level+3) 
		{skills[n]=skills[n]+3*(1+level/10);x=1;}
	}
}

void Character::skillselect(int s,int jobskills[],int num)
{		//randomly selects the character's skills based on job skill list
	int x,y,n;
	int *prev=new int[s];
	for(x=0;x<s;x++)
	{
		n=rand()%num;
		for(y=0;y<x;y++)
			if (prev[y]==n) n==35;
		if (n==35) continue;
		prev[x]=n;
		skills[jobskills[n]]=skills[jobskills[n]]+level+3;
	}
	delete prev;
}
	
void Character::feats(int lvl)	//randomly selects character's feats from level
{								//and feat requirements, if any
	int a,b,x,y,n;
	int *hist=new int[lvl/2+1];
	for(x=0;x<lvl/2;x++)
	{
		n=rand()%18;
		for (y=0;y<x;y++)
			if(hist[y]==n) n==100;
		if (n==100) continue;
		a=35;b=35;
		switch(n)									//feat name:
		{
			case 0: ;								//Great Fortitude
			case 1: ;								//Lightning Reflex
			case 2: saves[n]=saves[n]+2; break;		//Iron Will
			case 3: if(level<3) hp=hp+3;			//Toughness
					else  hp=hp+level; break;
			case 4: a=0; break;						//Acrobatic
			case 5: a=12;b=15; break;				//Alertness
			case 6: a=8;b=14; break;				//Animal Affinity
			case 7: a=3;b=20; break;				//Athletic
			case 8: a=2;b=6; break;					//Deceptive
			case 9: a=5;b=16; break;				//Nimble Fingers
			case 10: a=4;b=10; break;				//Diplomatic
			case 11: a=9;b=19; break;				//Self-Sufficient
			case 12: a=7;b=18; break;				//Stealthy
			case 13: if (ranged>melee) melee=ranged;
					 else continue; break;			//Weapon Finesse
			case 14: if (stats[2]>12) ac++;
					 else continue; break;			//Dodge
			case 15: if (stats[2]>11) (cmb+stats[2]-10)/2;
					 else continue; break;			//Agile Maneuvers
			case 16: init=init+4; break;			//Improved Initiative
			case 17: spd=spd+1; break;				//Fleet
			default: continue;
		}

	if (a!=35) skills[a]=skills[a]+2;			//if feat improves skills
	if (b!=35) skills[b]=skills[b]+2;			//raises those skills by two
	}

	delete hist;
}

void Character::raceselect()		//randomly selects the character's race
{
	int n;
	n=rand()%7;
	switch (n)
	{
		case 0: dwarf(); strcpy(race,"Dwarf"); break;
		case 1: elf(); strcpy(race,"Elf"); break;
		case 2: gnome(); strcpy(race,"Gnome"); break;
		case 3: helf(); strcpy(race,"Half-Elf"); break;
		case 4: horc(); strcpy(race,"Half-Orc"); break;
		case 5: halfling(); strcpy(race,"Halfling"); break;
		default: human(); strcpy(race,"Human"); break;
	}
}

void Character::dwarf()
{
	int n;

	stats[1]=stats[1]+2;	//dwarf gets +2 to Constitution
	stats[4]=stats[4]+2;	//and Wisdom
	stats[5]=stats[5]-2;	//and -2 to Charisma

	spd=spd*3/4;			//slower than taller races
	skills[1]=skills[1]+2;	//bonus to appraise

	n=rand()%12;
	switch (n)				//randomly selects dwarvish character name
	{
	case 0: strcpy(name,"Yangrit"); break;
	case 1: strcpy(name,"Dolgrin"); break;
	case 2: strcpy(name,"Grunyar"); break;
	case 3: strcpy(name,"Harsk"); break;
	case 4: strcpy(name,"Kazmuk"); break;
	case 5: strcpy(name,"Morgrym"); break;
	case 6: strcpy(name,"Rogar"); break;
	case 7: strcpy(name,"Agna"); break;
	case 8: strcpy(name,"Bodill"); break;
	case 9: strcpy(name,"Ingra"); break;
	case 10: strcpy(name,"Kotri"); break;
	case 11: strcpy(name,"Rusilka");break;
	}
}

void Character::elf()
{
	int n;

	stats[1]=stats[1]-2;	//elf gets -2 constitution
	stats[2]=stats[2]+2;	//and +2 dexterity
	stats[3]=stats[3]+2;	//and +2 intelligence

	skills[12]=skills[12]+2;	//bonus to perception

	n=rand()%17;	//randomly selects elvish character name
	switch (n)
	{
	case 0: strcpy(name,"Caladrel"); break;
	case 1: strcpy(name,"Heldalel"); break;
	case 2: strcpy(name,"Lanliss"); break;
	case 3: strcpy(name,"Meirdrarel"); break;
	case 4: strcpy(name,"Seldlon"); break;
	case 5: strcpy(name,"Talathel"); break;
	case 6: strcpy(name,"Variel"); break;
	case 7: strcpy(name,"Zordlon"); break;
	case 8: strcpy(name,"Amrunelara"); break;
	case 9: strcpy(name,"Dardlara"); break;
	case 10: strcpy(name,"Faunra"); break;
	case 11: strcpy(name,"Janthal");break;
	case 12: strcpy(name,"Merisiel"); break;
	case 13: strcpy(name,"Oparal"); break;
	case 14: strcpy(name,"Soumral"); break;
	case 15: strcpy(name,"Tessara"); break;
	case 16: strcpy(name,"Yalandlara"); break;
	}
}

void Character::gnome()
{
	int n;

	stats[0]=stats[0]-2;	//gnome gets -2 to strength
	stats[1]=stats[1]+2;	//and +2 to constitution
	stats[5]=stats[5]+2;	//and +2 to charisma

	ac=ac+1; spd=spd*3/4;	//smaller and slower than other races
	melee=melee+1; ranged=ranged+1;	//harder to hit and his other easier
	skills[18]=skills[18]+4;		//hard to see
	cmb=cmb-1; cmd=cmd-1;			//easier to knock over and grapple
	skills[12]=skills[12]+2;		//bonus to perception
	skills[32]=skills[32]+2;		//bonus to craft

	n=rand()%15;		//randomly selects gnomish name
	switch (n)
	{
	case 0: strcpy(name,"Abroshtor"); break;
	case 1: strcpy(name,"Bastargre"); break;
	case 2: strcpy(name,"Halungalom"); break;
	case 3: strcpy(name,"Krolmnite"); break;
	case 4: strcpy(name,"Poshment"); break;
	case 5: strcpy(name,"Zarzuket"); break;
	case 6: strcpy(name,"Zatqualmie"); break;
	case 7: strcpy(name,"Besh"); break;
	case 8: strcpy(name,"Fijit"); break;
	case 9: strcpy(name,"Lini"); break;
	case 10: strcpy(name,"Neji"); break;
	case 11: strcpy(name,"Majet");break;
	case 12: strcpy(name,"Pai"); break;
	case 13: strcpy(name,"Queck"); break;
	case 14: strcpy(name,"Trig"); break;
	}
}

void Character::helf()
{
	int x,n;

	statboost(2);		//half-elf gets a +2 to an appropriate attribute
	
	skills[12]=skills[12]+2;	//bonus to perception
	x=0;	
	while(x==0)		//half-elf gets a bonus to one skill
	{
		n=rand()%33;
		if (skills[n]>=level+3) 
		{skills[n]=skills[n]+3*(1+level/10);x=1;}
	}

	n=rand()%16;	//randomly selects a half-elf name
	switch (n)
	{
	case 0: strcpy(name,"Calathes"); break;
	case 1: strcpy(name,"Encinal"); break;
	case 2: strcpy(name,"Kyras"); break;
	case 3: strcpy(name,"Narciso"); break;
	case 4: strcpy(name,"Quiray"); break;
	case 5: strcpy(name,"Satinder"); break;
	case 6: strcpy(name,"Seltyiel"); break;
	case 7: strcpy(name,"Zirul"); break;
	case 8: strcpy(name,"Cathran"); break;
	case 9: strcpy(name,"Elsbeth"); break;
	case 10: strcpy(name,"Iandoli"); break;
	case 11: strcpy(name,"Kieyanna");break;
	case 12: strcpy(name,"Lialda"); break;
	case 13: strcpy(name,"Maddela"); break;
	case 14: strcpy(name,"Reda"); break;
	case 15: strcpy(name,"Tamarie"); break;
	}
}

void Character::horc()
{
	int n;

	statboost(2);	//half-orc gets a bonus to one attribute

	skills[10]=skills[10]+2;	//bonus to intimidate

	n=rand()%14;	//randomly selects half-orc name
	switch (n)
	{
	case 0: strcpy(name,"Ausk"); break;
	case 1: strcpy(name,"Davor"); break;
	case 2: strcpy(name,"Hakak"); break;
	case 3: strcpy(name,"Kizziar"); break;
	case 4: strcpy(name,"Makoa"); break;
	case 5: strcpy(name,"Nesteruk"); break;
	case 6: strcpy(name,"Tsadok"); break;
	case 7: strcpy(name,"Canan"); break;
	case 8: strcpy(name,"Drogheda"); break;
	case 9: strcpy(name,"Goruza"); break;
	case 10: strcpy(name,"Mazon"); break;
	case 11: strcpy(name,"Shirish");break;
	case 12: strcpy(name,"Tevaga"); break;
	case 13: strcpy(name,"Zeljika"); break;
	}
}

void Character::halfling()
{
	int n;
	
	stats[0]=stats[0]-2;	//halfling gets -2 to strength
	stats[2]=stats[2]+2;	//and a +2 to dexterity
	stats[5]=stats[5]+2;	//and a +2 to charisma

	ac=ac+1; spd=spd*3/4;	//smaller and slower than others
	melee=melee+1; ranged=ranged+1;	//harder to hit, hits other easier
	cmb=cmb-1; cmd=cmd-1;
	skills[18]=skills[18]+4;	//hard to see
	skills[12]=skills[12]+2;	//bonus to perception
	skills[0]=skills[0]+2;		//bonus to acrobatics
	skills[3]=skills[3]+2;		//bonus to climb
	saves[0]=saves[0]+1;saves[1]=saves[1]+1;saves[2]=saves[2]+1;
	//+1 to all saves
	n=rand()%17;	//randomly selects hafling name
	switch (n)
	{
	case 0: strcpy(name,"Antal"); break;
	case 1: strcpy(name,"Boram"); break;
	case 2: strcpy(name,"Evan"); break;
	case 3: strcpy(name,"Jamir"); break;
	case 4: strcpy(name,"Kaleb"); break;
	case 5: strcpy(name,"Lem"); break;
	case 6: strcpy(name,"Miro"); break;
	case 7: strcpy(name,"Sumak"); break;
	case 8: strcpy(name,"Anafa"); break;
	case 9: strcpy(name,"Bellis"); break;
	case 10: strcpy(name,"Etune"); break;
	case 11: strcpy(name,"Filiu");break;
	case 12: strcpy(name,"Lissa"); break;
	case 13: strcpy(name,"Marra"); break;
	case 14: strcpy(name,"Rillka"); break;
	case 15: strcpy(name,"Sistra"); break;
	case 16: strcpy(name,"Yamyra"); break;
	}
}

void Character::human()
{
	int x,n,s=1,num=33;	//one extra skill
	int jobskills[33];
	for (x=0;x<33;x++) jobskills[x]=x;	
	statboost(2);	//+2 to one attribute
	skillselect(s,jobskills,num);
	feats(1);	//one extra feat

	n=rand()%17;	//randomly selects one name
	switch (n)
	{
	case 0: strcpy(name,"Kim"); break;
	case 1: strcpy(name,"Victor"); break;
	case 2: strcpy(name,"Rolf"); break;
	case 3: strcpy(name,"Marcus"); break;
	case 4: strcpy(name,"Felix"); break;
	case 5: strcpy(name,"William"); break;
	case 6: strcpy(name,"Richard"); break;
	case 7: strcpy(name,"Gaius"); break;
	case 8: strcpy(name,"Andros"); break;
	case 9: strcpy(name,"Catherine"); break;
	case 10: strcpy(name,"Tilla"); break;
	case 11: strcpy(name,"Angeline");break;
	case 12: strcpy(name,"Patrice"); break;
	case 13: strcpy(name,"Robin"); break;
	case 14: strcpy(name,"Gwen"); break;
	case 15: strcpy(name,"Mari"); break;
	case 16: strcpy(name,"Nanase"); break;
	}
}

void Character::statboost(int bonus)
{	//improves one skill appropriate to character's job
	switch (job)
	{
		case EXPERT: stats[3]=stats[3]+bonus;break;
		case LEADER: stats[5]=stats[5]+bonus; break;
		case SCOUT: stats[2]=stats[2]+bonus; break;
		case CASTER: stats[4]=stats[4]+bonus; break;
		case WARRIOR: 
			if(stats[0]>stats[1]) stats[1]=stats[1]+bonus;
			else stats[0]=stats[0]+bonus;
			break;
		default: 
			if (stats[0]<stats[1] && stats[0]<stats[2]
				&& stats[0]<stats[3] && stats[0]<stats[4]
				&& stats[0]<stats[5]) stats[0]=stats[0]+bonus; 
			else if (stats[1]<stats[0] && stats[1]<stats[2]
				&& stats[1]<stats[3] && stats[1]<stats[4]
				&& stats[1]<stats[5]) stats[1]=stats[1]+bonus; 
			else if (stats[2]<stats[0] && stats[2]<stats[1]
				&& stats[2]<stats[3] && stats[2]<stats[4]
				&& stats[2]<stats[5]) stats[2]=stats[2]+bonus; 
			else if (stats[3]<stats[0] && stats[3]<stats[2]
				&& stats[3]<stats[1] && stats[3]<stats[4]
				&& stats[3]<stats[5]) stats[3]=stats[3]+bonus; 
			else if (stats[4]<stats[0] && stats[4]<stats[1]
				&& stats[4]<stats[3] && stats[4]<stats[2]
				&& stats[4]<stats[5]) stats[4]=stats[4]+bonus;
			else stats[5]=stats[5]+bonus;
	}
}

void Character::printchar()	//displays the character's vital statistics
{
	int x;	
	int skillist(int);

	cout<<name<<", "<<race;			//print name and race
	switch(job)						//print job
	{
	case WARRIOR: cout<<" Warrior"<<endl; break;
	case SCOUT: cout<<" Scout"<<endl; break;
	case EXPERT: cout<<" Expert"<<endl; break;
	case CASTER: cout<<" Caster"<<endl; break;
	case LEADER: cout<<" Aristocrat"<<endl; break;
	default: cout<<" Hireling"<<endl;
	}
	cout<<" Str "<<stats[0]<<" Con "<<stats[1]
		<<" Dex "<<stats[2]<<" Int "<<stats[3]
		<<" Wis "<<stats[4]<<" Cha "<<stats[5]<<endl;		//base attributes
	cout<<" HP: "<<hp<<" Spd: "<<spd<<" Init: "<<init<<endl;	
	cout<<" CMB: "<<cmb<<" CMD: "<<cmd<<endl;	//combat maneuvers
	cout<<" Fort "<<saves[0]<<" Ref "<<saves[1]
		<<" Will "<<saves[2]<<endl;		//saving throws
	weapons(); armor();		//character equipment
	cout<<" Healing Potions ("<<level/6+1<<"d8 + "<<level
		<<") x"<<level/2<<endl;	//number of heals available to character, scales by level
	for(x=0;x<33;x++)			//display chacter's skills
		if (skills[x]>0) 		//if character has skill in question
			cout<<(skills[x]+(stats[skillist(x)]-10)/2)<<" ";	//display name and skill score
	cout<<endl;
}

void Character::weapons() //randomly selects the character's weapons
{									//from those appropriate to class
	int n,meleebonus,rangedbonus;

	n=rand()%5;

	if (ranged<melee) 	//stronger character's favor melee
	{
		melee=melee+level/6;
		meleebonus=(stats[0]-10)/2+level/6;
		rangedbonus=0;
	}
	else 			//dextrous character favor ranged
	{
		ranged=ranged+level/6; 
		rangedbonus=level/6;
		meleebonus=(stats[0]-10)/2;
	}
	
	switch (job)
	{
	case HIRELING:
	case CASTER:
	case EXPERT: break;			//hireling, caster, expert all simple weapons
	case SCOUT: n=n+5; break;		//scout selects lighter weapons
	case LEADER: n=n+8; break;		//leader selects middle range of weapons
	case WARRIOR: n=n+10; break;	//warrior selects heavier weapons
	}

	cout<<" Melee "<<melee;

	switch (n)
	{
	case 0: cout<<" Knife 1d4 + "; break;
	case 1: cout<<" Sicle 1d6 + "; break;
	case 2: cout<<" Club 1d6 + "; break;
	case 3: cout<<" Spear 1d8 + "; break;
	case 4: cout<<" Staff 1d6 + "; break;
	case 5: cout<<" Hand Axe 1d6 + "; break;
	case 6: cout<<" Sicle 1d6 + "; break;
	case 7: cout<<" Hammer 1d4 + "; break;
	case 8: cout<<" Dagger 1d4 + "; break;
	case 9: cout<<" Rapier 1d6 + "; break;
	case 10: cout<<" Battle Axe 1d8 + "; ac++; break;
	case 11: cout<<" Sword 1d8 + "; ac++; break;
	case 12: cout<<" Flail 1d8 + "; ac++; break;
	case 13: cout<<" Great Axe 1d12 + ";  break;
	case 14: cout<<" Halberd 1d10 + "; break;
	}
	cout<<meleebonus<<endl;

	cout<<" Ranged "<<ranged;
	n=rand()%6;		//randomly selects one ranged weapon
	switch(n)
	{
	case 0: cout<<" Sling 1d4 + ";break;
	case 1: cout<<" Javelin 1d6 + ";break;
	case 2: cout<<" Crossbow 1d8 + ";break;
	case 3: cout<<" Shortbow 1d6 + ";break;
	case 4: cout<<" Longbow 1d8 + ";break;
	case 5: cout<<" Darts 1d4 + ";break;
	}

	cout<<rangedbonus<<endl;
}

void Character::armor()	//selects armor based on class
{
	int n;
	ac=ac+level/5;		//scales armor quality up based on level
	n=rand()%4+1;

cout<<" + "<<level/5;

switch (job)
	{
	case CASTER: 
	case HIRELING: n=0; break;	//caster and hireling wear no armor
	case EXPERT: 
	case SCOUT:  break;			//expert and scout wear light armor
	case LEADER: if(stats[2]>stats[0])
		{ac=ac+(stats[2]-10)/2; break;}
	case WARRIOR: n=n+4+level/2; break;	//leader and warrior wear heavy armor
	}

switch (n)
	{
	case 0: cout<<" Clothes "; ac=ac+0; break;
	case 1: cout<<" Padded "; ac=ac+1; break;
	case 2: cout<<" Leather "; ac=ac+2; break;
	case 3: cout<<" Studded "; ac=ac+3; break;
	case 4: cout<<" Chain Shirt "; ac=ac+4; break;
	case 5: cout<<" Hide "; 
			ac=ac+4; spd=spd-spd/3; break;		//heavy armor slows character
	case 6: cout<<" Scalemail "; 
			ac=ac+5; spd=spd-spd/3; break;
	case 7: cout<<" Chainmail "; 
			ac=ac+6; spd=spd-spd/3; break;
	case 8: cout<<" Breastplate "; 
			ac=ac+6; spd=spd-spd/3; break;
	case 9: cout<<" Splintmail "; 
			ac=ac+7; spd=spd-spd/3; break;
	case 10: cout<<" Banded "; 
			ac=ac+7; spd=spd-spd/3; break;
	case 11: cout<<" Half-Plate ";
			ac=ac+8; spd=spd-spd/3; break;
	default: cout<<" Full-Plate ";
			ac=ac+9; spd=spd-spd/3; break;
	}
cout<<" AC: "<<ac<<endl;		//armor class, measure of character's difficult to hit
}

int main()
{
	int x,members;

		srand(time(0));		//seed random number generator by system time
	
	cout<<"\nHow large a party? ";
	cin>>members;				//input number of character to be generated
	cout<<"\nWhat level? ";
	cin>>level;					//input level of characters

	Character *party=new Character[members];	//generate characters
	
	for(x=0;x<members;x++) party[x].printchar();	//print characters

	delete party;
	return 0;
}

int skillist(int entry)		//displays skill name and returns associated attribute
{
	int atr;

	switch (entry)
	{
		case 0: cout<<" Acrobatics "; atr=2; break;
		case 1: cout<<" Appraise "; atr=3; break;
		case 2: cout<<" Bluff "; atr=5; break;
		case 3: cout<<" Climb "; atr=0; break;
		case 4: cout<<" Diplomacy "; atr=5; break;
		case 5: cout<<" Disable Device "; atr=2; break;
		case 6: cout<<" Disguise "; atr=5; break;
		case 7: cout<<" Escape Artist "; atr=2; break;
		case 8: cout<<" Handle Animal "; atr=5; break;
		case 9: cout<<" Heal "; atr=4; break;
		case 10: cout<<" Intimidate "; atr=5; break;
		case 11: cout<<" Linguistics "; atr=3; break;
		case 12: cout<<" Perception "; atr=4; break;
		case 13: cout<<" Perform "; atr=5; break;
		case 14: cout<<" Ride "; atr=2; break;
		case 15: cout<<" Sense Motive "; atr=4; break;
		case 16: cout<<" Sleight of Hand "; atr=2; break;
		case 17: cout<<" Spellcraft "; atr=3; break;
		case 18: cout<<" Stealth "; atr=2; break;
		case 19: cout<<" Survival "; atr=4; break;
		case 20: cout<<" Swim "; atr=0; break;
		case 21: cout<<" Use Magic Device "; atr=5; break;
		case 22: cout<<" Know. Arcana "; atr=3; break;
		case 23: cout<<" Know. Dungeon "; atr=3; break;
		case 24: cout<<" Know. Engineering "; atr=3; break;
		case 25: cout<<" Know. Geography "; atr=3; break;
		case 26: cout<<" Know. History "; atr=3; break;
		case 27: cout<<" Know. Local "; atr=3; break;
		case 28: cout<<" Know. Nature "; atr=3; break;
		case 29: cout<<" Know. Nobility "; atr=3; break;
		case 30: cout<<" Know. Planes "; atr=3; break;
		case 31: cout<<" Know. Religion "; atr=2; break;
		case 32: cout<<" Craft "; atr=3; break;
	}
	return atr;
}

int max(int a, int b)	//returns larger of two integers
{
	if (a>b) return a;
	else return b;
}
