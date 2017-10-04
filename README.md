# Evolution
Model phenotypes using broad principals of evolutionary genetics.

AIM: To prepare a basic model for the genetic inheritance, reproduction and selection. Basically
model evolution.
Organism: The organism is called a Tril. It has following traits:
• Color: Which is controlled by R, G and B components
• Shape: Which is controlled by the three angles of the triangle.

class tril:

To control these traits, the organism has following genes:
• Red Gene: Controls the Red component.
• Green Gene: Controls the Green component.
• Blue Gene: Controls the Blue component.
• Distribution Gene: Controls the distribution of angles in the organism.
Every organism possesses a pair Genes for each trait mentioned.
One gene is chosen out of the pair at random to code for the phenotype.

	def __init__(self):
		self.r_gene=['' for x in range(2)]
		self.g_gene=['' for x in range(2)]
		self.b_gene=['' for x in range(2)]
		self.d_gene=['' for x in range(2)]
		self.color=(0,0,0)
		self.angles=(0,0,0)

Gene: It is a string made up of three characters(bases): A, B, C
These genes are converted to Proteins according to the following codon table:
AAA T
AAB T
AAC T
ABA T
ABB T
ABC S
ACA X
ACB X
ACC X
BAA H
BAB H
BAC H
BBA H
BBB H
BBC H
BCA H
BCB H
BCC H
CAA P
CAB P
CAC P
CBA Q
CBB Q
CBC Q
CCA R
CCB R
CCC R

	def translate(self,gene):
		prtn=''
		code ='''AAA T
			 AAB T
			AAC T
			ABA T
			ABB T
			ABC S
			ACA X
			ACB X
			ACC X
			BAA H
			BAB H
			BAC H
			BBA H
			BBB H
			BBC H
			BCA H
			BCB H
			BCC H
			CAA P
			CAB P
			CAC P
			CBA Q
			CBB Q
			CBC Q
			CCA R
			CCB R
			CCC R'''

		cd=code.split()
		codon = [cd[i] for i in range(len(cd)) if i%2==0]
		aa = [cd[i] for i in range(len(cd)) if i%2==1]
		codon_table=dict(zip(codon,aa))

		f=0

		for i in range(len(gene)):
			if gene[i:i+3]=='ABC':
				f=i
				break

		i=f

		while (True):
			if len(gene[i:i+3])<3 or i>len(gene):
				break

			prtn+=codon_table[(gene[i:i+3])]
			i+=3

		return (prtn)

ABC: S is treated as the start codon but there is no end codon.
Protein: It is a string starting with S and composed of characters (amino acids): H, P, Q, R, S, T,
X



The proportion of H’s in the string is normalized to a scale of 0-255 to obtain
color component.
[R/G/B] = 255*(No. of H’s in [R/G/B] Protein)/(Length of Protein)

	def ch_func(self,prtn):
		ch=0
		for aa in prtn:
			if aa=='H':
				ch+=1

		return int(255*(ch/len(prtn)))

The magnitude of the angle (P/Q/R) is proportional to the concentration of
P/Q/R in the protein.
Sum = No. of P + No. of Q + No. of R
Angle [P/Q/R] = 180*(No. of [P/Q/R] / Sum)

	def d_func(self,prtn):
		p,q,r,s=0,0,0,0

		for aa in prtn:
			if aa=='P': p+=1
			if aa=='Q': q+=1
			if aa=='R': r+=1

		s=p+q+r 

		if (s==0):
			return (60,60,60)
		else:
			return (180*(p/s),180*(q/s),180*(r/s))


	def pheno(self):
		import random as rd  

		r_prtn=self.translate(self.r_gene[rd.randint(0, 1)])
		g_prtn=self.translate(self.g_gene[rd.randint(0, 1)])
		b_prtn=self.translate(self.b_gene[rd.randint(0, 1)])

		d_prtn=self.translate(self.d_gene[rd.randint(0, 1)])

		r_phen=self.ch_func(r_prtn)
		g_phen=self.ch_func(g_prtn)
		b_phen=self.ch_func(b_prtn)

		self.angles=d_phen=self.d_func(d_prtn)

		self.color=color=(r_phen,g_phen,b_phen)


	def geno(self):
		for  i in range(2):
			print (self.r_gene[i])
			print (self.g_gene[i])
			print (self.b_gene[i])
			print (self.d_gene[i])


To begin with, a seed Population of P organism is generated. Genes of these organism are
generated randomly [The probability of occurrence of each base being 1/3].



	def genesis():
		import random

		proto=tril()


		cn={1:'A',2:'B',3:'C'}

		for j in range(2):
			for i in range(200):
				proto.r_gene[j]+=cn[(random.randint(1, 3))]
				proto.g_gene[j]+=cn[(random.randint(1, 3))]
				proto.b_gene[j]+=cn[(random.randint(1, 3))]
				proto.d_gene[j]+=cn[(random.randint(1, 3))]


		return (proto)
	
	
Each gene is composed of 2 strings. One inherited from Parent 1 and another from Parent 2.
Gene coming from a Parent:
An integer is chosen at random which is less than the length of the gene.
The new Gene String 1 = Gene1 of Parent 1 [ : k] + Gene2 of Parent 1 [k : ]
The new Gene String 2 = Gene1 of Parent 2 [ : k] + Gene2 of Parent 2 [k : ]


	def schwifty(sam,martha,n):
		import random

		k1,k2,l1,l2=0,0,0,0

		batman=[tril() for x in range(n)]

		for i in range(n):
			k1=random.randint(0, len(sam.r_gene[0]))
			l1=random.randint(0, 1)
			l2=random.randint(0, 1)
			batman[i].r_gene[l2]=sam.r_gene[l1][:k1]+sam.r_gene[1-l1][k1:]
			k2=random.randint(0, len(martha.r_gene[0]))
			l1=random.randint(0, 1)
			batman[i].r_gene[1-l2]=martha.r_gene[l1][:k2]+martha.r_gene[1-l1][k2:]

			k1=random.randint(0, len(sam.g_gene[0]))
			l1=random.randint(0, 1)
			l2=random.randint(0, 1)
			batman[i].g_gene[l2]=sam.g_gene[l1][:k1]+sam.g_gene[1-l1][k1:]
			k2=random.randint(0, len(martha.g_gene[0]))
			l1=random.randint(0, 1)
			batman[i].g_gene[1-l2]=martha.g_gene[l1][:k2]+martha.g_gene[1-l1][k2:]	

			k1=random.randint(0, len(sam.b_gene[0]))
			l1=random.randint(0, 1)
			l2=random.randint(0, 1)
			batman[i].b_gene[l2]=sam.b_gene[l1][:k1]+sam.b_gene[1-l1][k1:]
			k2=random.randint(0, len(martha.b_gene[0]))
			l1=random.randint(0, 1)
			batman[i].b_gene[1-l2]=martha.b_gene[l1][:k2]+martha.b_gene[1-l1][k2:]

			k1=random.randint(0, len(sam.d_gene[0]))
			l1=random.randint(0, 1)
			l2=random.randint(0, 1)
			batman[i].d_gene[l2]=sam.d_gene[l1][:k1]+sam.d_gene[1-l1][k1:]
			k2=random.randint(0, len(martha.d_gene[0]))
			l1=random.randint(0, 1)
			batman[i].d_gene[1-l2]=martha.d_gene[l1][:k2]+martha.d_gene[1-l1][k2:]

		return batman

	def fitness(sam):
		dev=0

		c=(130,45,198)

		for i in range(3):
			dev+=abs(sam.color[i]-c[i])

		return	dev


During mutation, every Nth [Mutagen Factor] base is replaced with another base which is
generated at random [The probability of occurrence of each base being 1/3].


	def mutate(sam):
		import random

		cn={1:'A',2:'B',3:'C'}

		Mutagen_Number=n=4

		musam=tril()

		for j in range(2):
			for char in sam.r_gene[j]:
				if random.randint(1,n)==n//2:
					musam.r_gene[j]+=cn[random.randint(1,3)]
				else:
					musam.r_gene[j]+=char

			for char in sam.g_gene[j]:
				if random.randint(1,n)==n//2:
					musam.g_gene[j]+=cn[random.randint(1,3)]
				else:
					musam.g_gene[j]+=char

			for char in sam.b_gene[j]:
				if random.randint(1,n)==n//2:
					musam.b_gene[j]+=cn[random.randint(1,3)]
				else:
					musam.b_gene[j]+=char

			for char in sam.d_gene[j]:
				if random.randint(1,n)==n//2:
					musam.d_gene[j]+=cn[random.randint(1,3)]
				else:
					musam.d_gene[j]+=char

		return (musam)

	P=100

	Mutation_Factor=0.1
	Parent_Factor=0.5

	GEN1=[genesis() for x in range(P)]

	for i in range(P):
		GEN1[i].pheno()

	PAR=[]
	GEN2=[]

	n=50

	for time in range(n):

These organism act as the population for the next cycle, but before the next cycle a certain
percentage of the population is mutated [Mutation Factor].

		for i in range(int(Mutation_Factor*P)):
			import random

			k=random.randint(0,P-1)
			GEN1[k]=mutate(GEN1[k])
			GEN1[k].pheno()

After this, the fitness of each organism is calculated from their phenotypic traits using a fitness
function. The organism is then segregated according to their fitness score.
A certain percentage [Parent Factor] of the fittest population is then allowed to reproduce,
taking organisms two at a time. As a result, nCr(P*Parent_Factor,2) organism are born.


		health=[fitness(GEN1[i]) for i in range(len(GEN1))]
		seg=sorted(health)
		print (seg)
	
		c=0
		for i in range(len(health)):
				if health[i]<=seg[int(Parent_Factor*P)]:
				PAR.append(GEN1[i])
				c+=1
			if c==int(Parent_Factor*P):
				break

		for i in range(len(PAR)):
			for j in range(i+1,len(PAR)):
				GEN2.append(schwifty(PAR[i],PAR[j],1)[0])

		for i in range(len(GEN2)):
			GEN2[i].pheno()

		GEN1=GEN2
		PAR=[]
		GEN2=[]
